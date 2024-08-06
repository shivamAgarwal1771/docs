import { useEffect, useMemo, useState, useCallback } from "react";
import { useTable, usePagination } from "react-table";
import Select from "./Select";
import { IoIosArrowBack, IoIosArrowForward } from "react-icons/io";
import NoDataAvailable from "./noDataAvailable";
import PrevHistoryLogo from "public/logo-previous-history.svg";
import { useRouter } from "next/router";
import { useAppContext } from "../common/appContext";

const pageSizeOptions = [
  { label: "5", value: "5" },
  { label: "10", value: "10" },
  { label: "15", value: "15" },
  { label: "20", value: "20" },
];
interface TableProps {
  limit: any;
  columns: any;
  setLimit: any;
  currentPage: any;
  totalPage: any;
  setCurrentPage: any;
  count: any;
  data: any;
  showPagination?: boolean;
  isCallTable?: boolean;
  isHistoryTable?: boolean;
}

const Table: React.FC<TableProps> = ({
  limit,
  setLimit,
  totalPage,
  currentPage,
  setCurrentPage,
  count,
  columns,
  data,
  showPagination,
  isCallTable,
  isHistoryTable,
}) => {
  const [datum, setDatum] = useState(data);
  const {
    getTableProps,
    getTableBodyProps,
    headerGroups,
    rows,
    prepareRow,
    canPreviousPage,
    canNextPage,
    pageOptions,
    nextPage,
    previousPage,
    setPageSize,
    state: { pageIndex, pageSize },
  } = useTable(
    {
      columns,
      data,
      initialState: { pageSize: limit, pageIndex: 0 },
    },
    usePagination
  );
  const { intlTranslator } = useAppContext()
  const router = useRouter();
  const [indexes, setIndexes] = useState(1);
  let dividedData;

  useEffect(() => {
    const sortedData = rows.sort((a, b) => (b.cells[6].value).localeCompare(a.cells[6].value));
    dividedData = Array.from({ length: Math.ceil(sortedData.length / 5) }, (data, i) => sortedData.slice(i * 5, i * 5 + 5));
    console.log("Divided Data: ", dividedData[indexes], rows);
  }, [rows, indexes])
  
  const decrementIndex = () => {
    setIndexes(indexes - 1)
  }

  const incrementIndex = () => {
    setIndexes(indexes + 1)
  }

  const defaultRowValue = useMemo(
    () =>
      pageSizeOptions.find((size) => size.value === String(limit)) || null,
    [limit]
  );

  useEffect(() => {
    if (currentPage + 1 == totalPage) { }
  }, [])

  const onPageClick = (event: any) => {
    console.log(currentPage)
    if (event.target.parentElement.id == 'left') {
      if (currentPage > 0) {
        console.log(currentPage)
        setCurrentPage(currentPage - 1)
      }
    }
    else {
      console.log(totalPage + ' ' + currentPage)
      if (totalPage != (currentPage + 1)) {
        setCurrentPage(currentPage + 1)
      }
    }
  }

  const urls = process.env.NEXT_PUBLIC_NEXTAUTH_URL;
  const smartAgent = useCallback((folderName: string) => {
    const smartAgentParams = new URLSearchParams({
      fileName: folderName,
    });
    const tab = window.open(`${urls}?${smartAgentParams}`);
    tab.onload = () => {
      window.location.reload();
      // dispatch(selectClient(""));
    };
  }, []);

  return (
    <div className="custom-react-table">
      <table {...getTableProps()} className="react-table">
        <thead className={`${isCallTable && "mycall-thead"}`}>
          {headerGroups.map((headerGroup) => (
            <tr {...headerGroup.getHeaderGroupProps()}>
              {headerGroup.headers.map((column) => (
                <td {...column.getHeaderProps()}>{column.render("Header")}</td>
              ))}

            </tr>
          ))}
        </thead>
        <tbody
          {...getTableBodyProps()}
          className={`${isHistoryTable && "history-tbody "}`}
        >
          {rows.map((row, i) => {
            prepareRow(row);
            return (
              <tr {...row.getRowProps()} className="table-row-css"
                onDoubleClick={() => { smartAgent(row.cells[0].value) }}
              >
                {row.cells.map((cell) => {
                  return (
                    <td {...cell.getCellProps()}>{cell.render("Cell")}</td>
                  );
                })}
              </tr>
            );
          })}
        </tbody>
      </table>
      {!data.length && (
        <div className="no-data-view">
          <NoDataAvailable
            img={PrevHistoryLogo}
            text={"No Previous Data Available"}
          />
        </div>
      )}

      {showPagination && (
        <div className="pagination">
          <div>
            <p>1-{limit} of {count}</p>
          </div>

          <div className="flex-container">
            <div className="flex-container">
              <p>Rows per page:</p>
              <Select
                options={pageSizeOptions}
                value={defaultRowValue}
                onChange={(e) => setLimit(Number(e.value))}
                width={75}
              />
            </div>
            <button id='left'
              onClick={() => decrementIndex()}
              // disabled={Math.max(prevIndex - 1, 0)}
              className="btn"
            >
              <IoIosArrowBack />
            </button>
            <p>
              <span className="pagenumber">{currentPage + 1}</span> /{" "}
              {totalPage}
            </p>
            <button id='right'
              onClick={() => incrementIndex()}
              // disabled={!canNextPage}
              className="btn"
            >
              <IoIosArrowForward />
            </button>
          </div>
        </div>
      )}
    </div>
  );
};

export default Table;
