import { useEffect, useMemo, useState } from "react";
import { useTable, usePagination } from "react-table";
import Select from "./Select";
import { IoIosArrowBack, IoIosArrowForward } from "react-icons/io";
import NoDataAvailable from "./noDataAvailable";
import PrevHistoryLogo from "public/logo-previous-history.svg";
import { useRouter } from "next/router";
import { useAppContext } from "../common/appContext"

const pageSizeOptions = [
  { label: "10", value: "10" },
  { label: "15", value: "15" },
  { label: "20", value: "20" },
];

interface TableProps {
  columns: any;
  data: any;
  showPagination?: boolean;
  isCallTable?: boolean;
  isHistoryTable?: boolean;
}

const Table: React.FC<TableProps> = ({
  columns,
  data,
  showPagination,
  isCallTable,
  isHistoryTable
}) => {
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
      initialState: { pageSize: 10, pageIndex: 0 },
    },
    usePagination
  );
  const { intlTranslator } = useAppContext()
  const router = useRouter();
  const defaultRowValue = useMemo(
    () =>
      pageSizeOptions.find((size) => size.value === String(pageSize)) || null,
    [pageSize]
  );

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
              <tr {...row.getRowProps()} className="table-row-css">
                {row.cells.map((cell) => {
                  return (
                    <td {...cell.getCellProps()}>{cell.column.id.includes("callId") || cell.column.id.includes("platform") || cell.column.id.includes("call_sentiment") || cell.column.id.includes("aqa") ? cell.render("Cell") : intlTranslator(cell.value)}</td>
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
            {/* <p>1-{limit} of {count}</p> */}
          </div>

          <div className="flex-container">
            <div className="flex-container">
              <p>Rows per page:</p>
              <Select
                options={pageSizeOptions}
                value={defaultRowValue}
                onChange={(e) => setPageSize(Number(e.value))}
                width={80}
              />
            </div>
            <button id='left'
              // onClick={event => onPageClick(event)}
              // disabled={!canPreviousPage}
              className="btn"
            >
              <IoIosArrowBack />
            </button>
            <p>
              <span className="pagenumber">{pageIndex + 1}</span> /{" "}
              {pageOptions.length}
            </p>
            <button id='right'
              // onClick={event => onPageClick(event)}
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
