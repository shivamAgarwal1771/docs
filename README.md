import React, { useState, useEffect } from "react";
import { useDispatch, useSelector } from "react-redux";
import { useOktaAuth } from "@okta/okta-react";
import AuthenticatedRoute from "../components/authenticatedRoute";
import ErrorUnAssignedUser from "../components/error/unassignedUser";
// import ConfigComponent from "../scripts/purecloud/main";
import { parseResponse } from "../utility";
import { updateUser, setIsLoading } from "../store/userSlice";
import { GET_USER_BY_ID } from "../graphql/userMicroservice/queries";
import client from "../apollo-client";
import { useRouter } from "next/router";
import { BYPASS_OKTA_LOGIN, SIMULATION_USER } from "../utility/constants";
const MainComponent = () => {
  const { oktaAuth, authState } = useOktaAuth();
  // const appUser = useSelector((state) => state.user);
  const [unAssignedUser, setUnAssignedUser] = useState(false);
  const dispatch = useDispatch();
  const router = useRouter();

  const [oktaUser, setOktaUser] = useState(null);

  const getUserByIDQueryCall = async () => {
    const [error, response] = await parseResponse(
      client.query({
        query: GET_USER_BY_ID,
        variables: { oktaId: oktaUser?.sub || "" },
      })
    );
    if (error) {
      console.error("getUserByIDQueryCall : error ==> ", error);
      return;
    }
    if (response) {
      console.log("Logged IN user: ", response?.data?.user);
      dispatch(updateUser(response?.data?.user));
    }
  };

  useEffect(() => {
    if (oktaUser?.sub && !BYPASS_OKTA_LOGIN) {
      getUserByIDQueryCall();
    }
  }, [oktaUser]);

  const getUserInfo = async () => {
    const [OktaGetUserInfoError, OktaGetUserInfoResponse] = await parseResponse(
      oktaAuth.getUser()
    );
    if (OktaGetUserInfoError) {
      console.error("OktaGetUserInfoError ==> ", OktaGetUserInfoError);
      // TODO Add PopUP
      return;
    }
    setOktaUser(OktaGetUserInfoResponse);
  };

  const RedirectToLoginPage = () => {
    router.push("/login");
    return null;
  };

  useEffect(() => {
    if (
      (oktaAuth && authState && authState.isAuthenticated) ||
      !BYPASS_OKTA_LOGIN
    ) {
      getUserInfo();
    }
  }, [authState]);

  // if (authState && !authState.isAuthenticated && !BYPASS_OKTA_LOGIN) {
  //   return <RedirectToLoginPage />;
  // }

  // if ((!authState || !oktaAuth) && !BYPASS_OKTA_LOGIN) {
  //   return <div className="loading-view">Loading...</div>;
  // }

  useEffect(() => {
    if (BYPASS_OKTA_LOGIN) {
      dispatch(updateUser(SIMULATION_USER));
    }
  }, []);

  return (
    <div>
      {/* <ConfigComponent /> */}
      {!BYPASS_OKTA_LOGIN ? (
        <ErrorUnAssignedUser />
      ) : BYPASS_OKTA_LOGIN ? (
        <AuthenticatedRoute />
      ) : // <RedirectToLoginPage />
      null}
    </div>
  );
};

export default MainComponent;
