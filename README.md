import Table from "../../components/common/Table";
import { useEffect, useRef, useState, useMemo, useCallback } from "react";
import Image from "next/future/image";
import { useOktaAuth } from "@okta/okta-react";
import clientList from "../../assets/resource/client-list.json";
import { useDispatch, useSelector } from "react-redux";
import { selectClient, setHeader } from "../../store/userSlice"
// import { signOut } from "../../store/userSlice";
// import { parseResponse } from "../../utility";
import Router, { useRouter } from "next/router";
// import { resetCallState } from "../../store/callSlice";
import { CALL_AVAILABLE_STATE, BYPASS_OKTA_LOGIN, DEMO_TABLE_HEADERS } from "../../utility/constants";
import { FaPlay } from "react-icons/fa";
import { useAppContext } from "../../components/common/appContext"

interface MyCallScreenProps {
    isDashboard?: boolean;
}
const MyCallScreen: React.FC<MyCallScreenProps> = ({ isDashboard }) => {
    const { intlTranslator } = useAppContext()
    const dispatch = useDispatch();
    const selectedHeader = useSelector((state: any) => state?.user);
    const selectedClient = (values) => {
        dispatch(setHeader([values.header, values.subHeader]))
        const finalClientData = (values.agent + "_" + values.version).toLowerCase(); // key for specific client data
        dispatch(selectClient(finalClientData))
    }
    const urls = process.env.NEXT_PUBLIC_NEXTAUTH_URL;
    const smartAgent = useCallback((name: string, version: string) => {
        const smartAgentParams = new URLSearchParams({
            clientName: name,
            version: version
        }).toString();
        const tab = window.open(`${urls}?${smartAgentParams}`,'_blank')
        tab.onload = () => {
            window.location.reload()
            dispatch(selectClient(""))
        }

    }, []);
    const columns = useMemo(
        () => [
            {
                Header: intlTranslator(DEMO_TABLE_HEADERS.CALL_ID),
                accessor: "callId",
                Cell: ({ cell: { value } }) => (
                    <div className="cell_align">
                        <p className="cell_style">{btoa(value)}</p>
                    </div>
                ),
            },
            {
                Header: intlTranslator(DEMO_TABLE_HEADERS.AHT),
                accessor: "aht",
                Cell: ({ cell: { value } }) => (
                    <p className="cell_style">{value}</p>
                ),
            },
            {
                Header: intlTranslator(DEMO_TABLE_HEADERS.AGENT),
                accessor: "agent",
                Cell: ({ cell: { value } }) => (
                    <p className="cell_style">{value}</p>
                ),
            },
            {
                Header: intlTranslator(DEMO_TABLE_HEADERS.USECASE),
                accessor: "useCase",
                Cell: ({ cell: { value } }) => (
                    <p className="cell_style">{value}</p>
                ),
            },
            {
                Header: intlTranslator(DEMO_TABLE_HEADERS.INDUSTRY),
                accessor: "industry",
                Cell: ({ cell: { value } }) => (
                    <p className="cell_style">{value}</p>
                ),
            },
            {
                Header: intlTranslator(DEMO_TABLE_HEADERS.CHANNEL),
                accessor: "channel",
                Cell: ({ cell: { value } }) => (
                    <p className="cell_style"> {value}</p>
                ),
            },
            {
                Header: intlTranslator(DEMO_TABLE_HEADERS.INTERACTION_DATE),
                accessor: "date",
                Cell: ({ cell: { value } }) => (
                    <p className="cell_style">{value}</p>
                ),
            },
            {
                Header: intlTranslator(DEMO_TABLE_HEADERS.LAUNCH),
                accessor: "platform",
                Cell: ({ cell: { value } }) => (
                    <FaPlay onClick={() => smartAgent(value.client, value.version)} className="button_style" />
                ),
            },
        ],
        [intlTranslator]
    );

    const filteredColumns = useMemo(
        () =>
            isDashboard
                ? columns
                    .map((column) => {
                        if (column.Header !== "Call ID") return column;
                        return <td style={{ textAlign: "start" }}></td>;
                    })
                    .filter((col) => col)
                : columns,
        [columns, isDashboard]
    );

    return (
        <div style={{ padding: "1rem" }} >
            <Table
                data={clientList}
                columns={filteredColumns}
                showPagination={!isDashboard}
                isCallTable
                clientListData={selectedClient}
            />
        </div>
    );
};


export default function ClientList() {
    const { oktaAuth, authState } = useOktaAuth();
    const availableStateRef = useRef(null);
    const dispatch = useDispatch();
    const router = useRouter();
    // const appUser = useSelector((state) => state.user);
    // const appCallData = useSelector((state) => state.call);
    const [tabValue, setTabValue] = useState<number>(0);

    const [isAvailable, setIsAvailable] = useState(false);
    const [availableState, setAvailableState] = useState(
        CALL_AVAILABLE_STATE.READY
    );
    const [showAvailableState, setShowAvailableState] = useState(false);

    return (
        <>
            <div className="page_container">

                <main>
                    {tabValue === 0 && <MyCallScreen />}
                </main>

            </div>
        </>
    );
}
