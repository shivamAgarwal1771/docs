import React, { useState, useEffect } from "react";
import { useDispatch, useSelector } from "react-redux";
// import { useOktaAuth } from "@okta/okta-react";
import AuthenticatedRoute from "../components/authenticatedRoute";
import ErrorUnAssignedUser from "../components/error/unassignedUser";
import ConfigComponent from "../scripts/purecloud/main";
import { parseResponse } from "../utility";
import { updateUser, setIsLoading } from "../store/userSlice";
import { GET_USER_BY_ID } from "../graphql/userMicroservice/queries";
import client from "../apollo-client";
import { useRouter } from "next/router";
// import { getSession, useSession } from "next-auth/react"
import { setCookie } from "cookies-next";
import { CTI_PROVIDER } from "../utility/constants";
import { setCognitoAccessToken } from "../store/userSlice";

const MainComponent = () => {
  // const { oktaAuth, authState } = useOktaAuth();
  const [unAssignedUser, setUnAssignedUser] = useState(false);
  const dispatch = useDispatch();
  const router = useRouter();
  const appUser = useSelector((state: any) => state.user);
  // const { data: session, status } = useSession();
  const configuredCTI = process.env.NEXT_PUBLIC_CONFIGURED_CTI;

  const [oktaUser, setOktaUser] = useState(null);
  const [isAppVerified, useIsAppVerified] = useState(false);
  // const [session, setSession] = useState(null);
  const [loading, setLoading] = useState(true);
  const [accessToken, setAccessToken] = useState(null);

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
      dispatch(updateUser(response?.data?.user));
    }
  };

  function getParameterByName(name) {
    name = name.replace(/[\\[]/, "\\[").replace(/[\]]/, "\\]");
    var regex = new RegExp("[\\#&]" + name + "=([^&#]*)"),
      results = regex.exec(location.hash);
    return results === null
      ? ""
      : decodeURIComponent(results[1].replace(/\+/g, " "));
  }

  useEffect(() => {
    if (location.hash) {
      setLoading(true);
      const token = getParameterByName("access_token");
      if (token) {
        setAccessToken(token);
        dispatch(setCognitoAccessToken(token));
        // Store tokens in cookies
        setCookie("access_token", token);
        setLoading(false);
      }
      if(!token){
        router.push("/login");
      }
    } else {
      router.push("/login");
    }
  }, []);

  // useEffect(() => {
  //   const fetchSession = async () => {
  //     const session = await getSession();
  //     console.log("getSession", session)
  //     setSession(session);
  //     setLoading(false);
  //   };
  //   fetchSession();
  // }, []);

  // useEffect(() => {
  //   if (oktaUser?.sub) {
  //     getUserByIDQueryCall();
  //   }
  // }, [oktaUser]);

  // const getUserInfo = async () => {
  //   const [OktaGetUserInfoError, OktaGetUserInfoResponse] = await parseResponse(
  //     oktaAuth.getUser()
  //   );
  //   if (OktaGetUserInfoError) {
  //     console.error("OktaGetUserInfoError ==> ", OktaGetUserInfoError);
  //     // TODO Add PopUP
  //     return;
  //   }
  //   setOktaUser(OktaGetUserInfoResponse);
  // };

  const RedirectToLoginPage = () => {
    router.push("/login");
    return null;
  };

  // useEffect(() => {
  //   if (oktaAuth && authState && authState.isAuthenticated) {
  //     getUserInfo();
  //   }
  // }, [authState]);

  if (loading) {
    return <div className="loading-view">Loading...</div>;
  }

  return (
    <>
      {appUser.cognitoAccessToken && (
        <>
          {configuredCTI === CTI_PROVIDER.GENESYS_PURE_CLOUD && accessToken && (
            <ConfigComponent />
          )}
          <AuthenticatedRoute />
        </>
      )}
      {/* {configuredCTI === CTI_PROVIDER.GENESYS_PURE_CLOUD && <ConfigComponent />}
      {unAssignedUser ? (
        <ErrorUnAssignedUser />
      ) : authState && authState.isAuthenticated ? (
        <AuthenticatedRoute />
      ) : (
        <RedirectToLoginPage />
      )} */}
    </>
  );
};

export default MainComponent;
