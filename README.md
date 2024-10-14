import "bootstrap/dist/css/bootstrap.css";
import "bootstrap/dist/css/bootstrap.min.css";
import "../assets/css/common.css";
import "../assets/css/callmodal.css";
import "../assets/css/languages.css";
// import "../assets/css/calldetails.css";
import "../assets/css/chatDetails.css";
import "../assets/css/reacttable.css";
import "../assets/css/base-layout.css";
import "../assets/css/ai-assistant.css";
import "../assets/css/action-workflow.css";
import "../assets/css/callWidget.css";
import "../assets/css/sentiment.css";
import "../assets/css/transcript.css";
import "../assets/css/contactInformation.css";
import "../assets/css/cases.css";
import "../assets/css/editCase.css";
import "../assets/css/interactionHistory.css";
import "../assets/css/createCase.css";
import "../assets/css/auto-audit.css";
import "../assets/css/callsummary.css";
import "../assets/css/ai-wiki.css";
import "../assets/css/dashboard.css";
import "../assets/css/customer-360.css";
import "../assets/css/demoPage.css";
import "../assets/css/callContext.css";
import "../assets/css/automateApp.css";
import "../assets/css/globals.css";
import "../assets/css/custom.css";

import Head from "next/head";
import { useEffect } from "react";
import { SessionProvider } from "next-auth/react";
import { Provider } from "react-redux";
import { OktaAuth, toRelativeUrl } from "@okta/okta-auth-js";
import Router from "next/router";
import { store, persistor } from "../store";
import { IntlProviderWrapper } from '../components/IntlProviderWrapper';
import ApplicationLayoutHOC from "../components/hoc/applicationLayoutHOC";
import "../utility/authentication/amplify-config";
import ErrorBoundary from "../components/error/errorBoundary";
import { ApolloProvider } from "@apollo/client";
import client from "../apollo-client";
import { PersistGate } from "redux-persist/integration/react";
import logger from '../helpers/logger';
import '../styles/globals.css';

if (typeof window !== "undefined") {
  require("amazon-connect-streams");
}

const oktaAuthClient = new OktaAuth({
  issuer: process.env.NEXT_PUBLIC_OKTA_ISSUER || "",
  clientId: process.env.OKTA_CLIENT_ID || "",
  redirectUri: "/",
});

function MyApp({ Component, pageProps }) {
  useEffect(() => {
    typeof document !== undefined
      ? require("bootstrap/dist/js/bootstrap")
      : null;

    typeof document !== undefined ? require("moment/moment.js") : null;

    logger.error('My App initialized');
  }, []);

  const restoreOriginalUri = async (_oktaAuth, originalUri) => {
    Router.replace("/");
  };

  return (
    <ErrorBoundary>
      <Provider store={store}>
        <PersistGate
          loading={<div className="loading-view">Loading...</div>}
          persistor={persistor}
        >
          {/* <Security
            oktaAuth={oktaAuthClient}
            restoreOriginalUri={restoreOriginalUri}
          > */}
          <SessionProvider session={pageProps.session}>
            <ApolloProvider client={client}>
              <IntlProviderWrapper>
                <Head>
                  <title>Agent Assist</title>
                  <meta
                    name="viewport"
                    content="width=device-width, initial-scale=1"
                  />
                </Head>
                <ApplicationLayoutHOC>
                  <Component {...pageProps} />
                </ApplicationLayoutHOC>
              </IntlProviderWrapper>
            </ApolloProvider>
          </SessionProvider>
          {/* </Security> */}
        </PersistGate>
      </Provider>
    </ErrorBoundary>
  );
}

MyApp.getInitialProps = async (ctx) => {
  const areLogsEnabled = ctx?.router?.query?.debug || '';
  global.areLogsEnabled = areLogsEnabled === 'true';
  return {};
};

export default MyApp;
