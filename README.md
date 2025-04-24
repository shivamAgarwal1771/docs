ERROR in src/visualizations/TimeTable/TimeTable.jsx:297:17
prettier/prettier: Delete `␍`
    295 |           };
    296 |         }
  > 297 |         return {
        |                 ^
    298 |           ...acc,
    299 |           [columnConfig.key]: renderValueCell(
    300 |             valueField,

ERROR in src/visualizations/TimeTable/TimeTable.jsx:298:18
prettier/prettier: Delete `␍`
    296 |         }
    297 |         return {
  > 298 |           ...acc,
        |                  ^
    299 |           [columnConfig.key]: renderValueCell(
    300 |             valueField,
    301 |             columnConfig,

ERROR in src/visualizations/TimeTable/TimeTable.jsx:299:47
prettier/prettier: Delete `␍`
    297 |         return {
    298 |           ...acc,
  > 299 |           [columnConfig.key]: renderValueCell(
        |                                               ^
    300 |             valueField,
    301 |             columnConfig,
    302 |             reversedEntries,

ERROR in src/visualizations/TimeTable/TimeTable.jsx:300:24
prettier/prettier: Delete `␍`
    298 |           ...acc,
    299 |           [columnConfig.key]: renderValueCell(
  > 300 |             valueField,
        |                        ^
    301 |             columnConfig,
    302 |             reversedEntries,
    303 |           ),

ERROR in src/visualizations/TimeTable/TimeTable.jsx:301:26
prettier/prettier: Delete `␍`
    299 |           [columnConfig.key]: renderValueCell(
    300 |             valueField,
  > 301 |             columnConfig,
        |                          ^
    302 |             reversedEntries,
    303 |           ),
    304 |         };

ERROR in src/visualizations/TimeTable/TimeTable.jsx:302:29
prettier/prettier: Delete `␍`
    300 |             valueField,
    301 |             columnConfig,
  > 302 |             reversedEntries,
        |                             ^
    303 |           ),
    304 |         };
    305 |       }, {});

ERROR in src/visualizations/TimeTable/TimeTable.jsx:303:13
prettier/prettier: Delete `␍`
    301 |             columnConfig,
    302 |             reversedEntries,
  > 303 |           ),
        |             ^
    304 |         };
    305 |       }, {});
    306 |       return { ...row, ...cellValues, metric: renderLeftCell(row) };

ERROR in src/visualizations/TimeTable/TimeTable.jsx:304:11
prettier/prettier: Delete `␍`
    302 |             reversedEntries,
    303 |           ),
  > 304 |         };
        |           ^
    305 |       }, {});
    306 |       return { ...row, ...cellValues, metric: renderLeftCell(row) };
    307 |     });

ERROR in src/visualizations/TimeTable/TimeTable.jsx:305:14
prettier/prettier: Delete `␍`
    303 |           ),
    304 |         };
  > 305 |       }, {});
        |              ^
    306 |       return { ...row, ...cellValues, metric: renderLeftCell(row) };
    307 |     });
    308 |   }, [columnConfigs, data, rowType, rows, url]);

ERROR in src/visualizations/TimeTable/TimeTable.jsx:306:69
prettier/prettier: Delete `␍`
    304 |         };
    305 |       }, {});
  > 306 |       return { ...row, ...cellValues, metric: renderLeftCell(row) };
        |                                                                     ^
    307 |     });
    308 |   }, [columnConfigs, data, rowType, rows, url]);
    309 |

ERROR in src/visualizations/TimeTable/TimeTable.jsx:307:8
prettier/prettier: Delete `␍`
    305 |       }, {});
    306 |       return { ...row, ...cellValues, metric: renderLeftCell(row) };
  > 307 |     });
        |        ^
    308 |   }, [columnConfigs, data, rowType, rows, url]);
    309 |
    310 |   const defaultSort =

ERROR in src/visualizations/TimeTable/TimeTable.jsx:308:49
prettier/prettier: Delete `␍`
    306 |       return { ...row, ...cellValues, metric: renderLeftCell(row) };
    307 |     });
  > 308 |   }, [columnConfigs, data, rowType, rows, url]);
        |                                                 ^
    309 |
    310 |   const defaultSort =
    311 |     rowType === 'column' && columnConfigs.length

ERROR in src/visualizations/TimeTable/TimeTable.jsx:309:1
prettier/prettier: Delete `␍`
    307 |     });
    308 |   }, [columnConfigs, data, rowType, rows, url]);
  > 309 |
        | ^
    310 |   const defaultSort =
    311 |     rowType === 'column' && columnConfigs.length
    312 |       ? [

ERROR in src/visualizations/TimeTable/TimeTable.jsx:310:22
prettier/prettier: Delete `␍`
    308 |   }, [columnConfigs, data, rowType, rows, url]);
    309 |
  > 310 |   const defaultSort =
        |                      ^
    311 |     rowType === 'column' && columnConfigs.length
    312 |       ? [
    313 |           {

ERROR in src/visualizations/TimeTable/TimeTable.jsx:311:49
prettier/prettier: Delete `␍`
    309 |
    310 |   const defaultSort =
  > 311 |     rowType === 'column' && columnConfigs.length
        |                                                 ^
    312 |       ? [
    313 |           {
    314 |             id: columnConfigs[0].key,

ERROR in src/visualizations/TimeTable/TimeTable.jsx:312:10
prettier/prettier: Delete `␍`
    310 |   const defaultSort =
    311 |     rowType === 'column' && columnConfigs.length
  > 312 |       ? [
        |          ^
    313 |           {
    314 |             id: columnConfigs[0].key,
    315 |             desc: 'true',

ERROR in src/visualizations/TimeTable/TimeTable.jsx:313:12
prettier/prettier: Delete `␍`
    311 |     rowType === 'column' && columnConfigs.length
    312 |       ? [
  > 313 |           {
        |            ^
    314 |             id: columnConfigs[0].key,
    315 |             desc: 'true',
    316 |           },

ERROR in src/visualizations/TimeTable/TimeTable.jsx:314:38
prettier/prettier: Delete `␍`
    312 |       ? [
    313 |           {
  > 314 |             id: columnConfigs[0].key,
        |                                      ^
    315 |             desc: 'true',
    316 |           },
    317 |         ]

ERROR in src/visualizations/TimeTable/TimeTable.jsx:315:26
prettier/prettier: Delete `␍`
    313 |           {
    314 |             id: columnConfigs[0].key,
  > 315 |             desc: 'true',
        |                          ^
    316 |           },
    317 |         ]
    318 |       : [];

ERROR in src/visualizations/TimeTable/TimeTable.jsx:316:13
prettier/prettier: Delete `␍`
    314 |             id: columnConfigs[0].key,
    315 |             desc: 'true',
  > 316 |           },
        |             ^
    317 |         ]
    318 |       : [];
    319 |

ERROR in src/visualizations/TimeTable/TimeTable.jsx:317:10
prettier/prettier: Delete `␍`
    315 |             desc: 'true',
    316 |           },
  > 317 |         ]
        |          ^
    318 |       : [];
    319 |
    320 |   return (

ERROR in src/visualizations/TimeTable/TimeTable.jsx:318:12
prettier/prettier: Delete `␍`
    316 |           },
    317 |         ]
  > 318 |       : [];
        |            ^
    319 |
    320 |   return (
    321 |     <TimeTableStyles

ERROR in src/visualizations/TimeTable/TimeTable.jsx:319:1
prettier/prettier: Delete `␍`
    317 |         ]
    318 |       : [];
  > 319 |
        | ^
    320 |   return (
    321 |     <TimeTableStyles
    322 |       data-test="time-table"

ERROR in src/visualizations/TimeTable/TimeTable.jsx:320:11
prettier/prettier: Delete `␍`
    318 |       : [];
    319 |
  > 320 |   return (
        |           ^
    321 |     <TimeTableStyles
    322 |       data-test="time-table"
    323 |       className={className}

ERROR in src/visualizations/TimeTable/TimeTable.jsx:321:21
prettier/prettier: Delete `␍`
    319 |
    320 |   return (
  > 321 |     <TimeTableStyles
        |                     ^
    322 |       data-test="time-table"
    323 |       className={className}
    324 |       height={height}

ERROR in src/visualizations/TimeTable/TimeTable.jsx:322:29
prettier/prettier: Delete `␍`
    320 |   return (
    321 |     <TimeTableStyles
  > 322 |       data-test="time-table"
        |                             ^
    323 |       className={className}
    324 |       height={height}
    325 |     >

ERROR in src/visualizations/TimeTable/TimeTable.jsx:323:28
prettier/prettier: Delete `␍`
    321 |     <TimeTableStyles
    322 |       data-test="time-table"
  > 323 |       className={className}
        |                            ^
    324 |       height={height}
    325 |     >
    326 |       <TableView

ERROR in src/visualizations/TimeTable/TimeTable.jsx:324:22
prettier/prettier: Delete `␍`
    322 |       data-test="time-table"
    323 |       className={className}
  > 324 |       height={height}
        |                      ^
    325 |     >
    326 |       <TableView
    327 |         className="table-no-hover"

ERROR in src/visualizations/TimeTable/TimeTable.jsx:325:6
prettier/prettier: Delete `␍`
    323 |       className={className}
    324 |       height={height}
  > 325 |     >
        |      ^
    326 |       <TableView
    327 |         className="table-no-hover"
    328 |         columns={memoizedColumns}

ERROR in src/visualizations/TimeTable/TimeTable.jsx:326:17
prettier/prettier: Delete `␍`
    324 |       height={height}
    325 |     >
  > 326 |       <TableView
        |                 ^
    327 |         className="table-no-hover"
    328 |         columns={memoizedColumns}
    329 |         data={memoizedRows}

ERROR in src/visualizations/TimeTable/TimeTable.jsx:327:35
prettier/prettier: Delete `␍`
    325 |     >
    326 |       <TableView
  > 327 |         className="table-no-hover"
        |                                   ^
    328 |         columns={memoizedColumns}
    329 |         data={memoizedRows}
    330 |         initialSortBy={defaultSort}

ERROR in src/visualizations/TimeTable/TimeTable.jsx:328:34
prettier/prettier: Delete `␍`
    326 |       <TableView
    327 |         className="table-no-hover"
  > 328 |         columns={memoizedColumns}
        |                                  ^
    329 |         data={memoizedRows}
    330 |         initialSortBy={defaultSort}
    331 |         withPagination={false}

ERROR in src/visualizations/TimeTable/TimeTable.jsx:329:28
prettier/prettier: Delete `␍`
    327 |         className="table-no-hover"
    328 |         columns={memoizedColumns}
  > 329 |         data={memoizedRows}
        |                            ^
    330 |         initialSortBy={defaultSort}
    331 |         withPagination={false}
    332 |       />

ERROR in src/visualizations/TimeTable/TimeTable.jsx:330:36
prettier/prettier: Delete `␍`
    328 |         columns={memoizedColumns}
    329 |         data={memoizedRows}
  > 330 |         initialSortBy={defaultSort}
        |                                    ^
    331 |         withPagination={false}
    332 |       />
    333 |     </TimeTableStyles>

ERROR in src/visualizations/TimeTable/TimeTable.jsx:331:31
prettier/prettier: Delete `␍`
    329 |         data={memoizedRows}
    330 |         initialSortBy={defaultSort}
  > 331 |         withPagination={false}
        |                               ^
    332 |       />
    333 |     </TimeTableStyles>
    334 |   );

ERROR in src/visualizations/TimeTable/TimeTable.jsx:332:9
prettier/prettier: Delete `␍`
    330 |         initialSortBy={defaultSort}
    331 |         withPagination={false}
  > 332 |       />
        |         ^
    333 |     </TimeTableStyles>
    334 |   );
    335 | };

ERROR in src/visualizations/TimeTable/TimeTable.jsx:333:23
prettier/prettier: Delete `␍`
    331 |         withPagination={false}
    332 |       />
  > 333 |     </TimeTableStyles>
        |                       ^
    334 |   );
    335 | };
    336 |

ERROR in src/visualizations/TimeTable/TimeTable.jsx:334:5
prettier/prettier: Delete `␍`
    332 |       />
    333 |     </TimeTableStyles>
  > 334 |   );
        |     ^
    335 | };
    336 |
    337 | TimeTable.propTypes = propTypes;

ERROR in src/visualizations/TimeTable/TimeTable.jsx:335:3
prettier/prettier: Delete `␍`
    333 |     </TimeTableStyles>
    334 |   );
  > 335 | };
        |   ^
    336 |
    337 | TimeTable.propTypes = propTypes;
    338 | TimeTable.defaultProps = defaultProps;

ERROR in src/visualizations/TimeTable/TimeTable.jsx:336:1
prettier/prettier: Delete `␍`
    334 |   );
    335 | };
  > 336 |
        | ^
    337 | TimeTable.propTypes = propTypes;
    338 | TimeTable.defaultProps = defaultProps;
    339 |

ERROR in src/visualizations/TimeTable/TimeTable.jsx:337:33
prettier/prettier: Delete `␍`
    335 | };
    336 |
  > 337 | TimeTable.propTypes = propTypes;
        |                                 ^
    338 | TimeTable.defaultProps = defaultProps;
    339 |
    340 | export default TimeTable;

ERROR in src/visualizations/TimeTable/TimeTable.jsx:338:39
prettier/prettier: Delete `␍`
    336 |
    337 | TimeTable.propTypes = propTypes;
  > 338 | TimeTable.defaultProps = defaultProps;
        |                                       ^
    339 |
    340 | export default TimeTable;
    341 |

ERROR in src/visualizations/TimeTable/TimeTable.jsx:339:1
prettier/prettier: Delete `␍`
    337 | TimeTable.propTypes = propTypes;
    338 | TimeTable.defaultProps = defaultProps;
  > 339 |
        | ^
    340 | export default TimeTable;
    341 |

ERROR in src/visualizations/TimeTable/TimeTable.jsx:340:26
prettier/prettier: Delete `␍`
    338 | TimeTable.defaultProps = defaultProps;
    339 |
  > 340 | export default TimeTable;
        |                          ^
    341 |

ERROR in src/visualizations/TimeTable/transformProps.ts:1:4
prettier/prettier: Delete `␍`
  > 1 | /**
      |    ^
    2 |  * Licensed to the Apache Software Foundation (ASF) under one
    3 |  * or more contributor license agreements.  See the NOTICE file
    4 |  * distributed with this work for additional information

ERROR in src/visualizations/TimeTable/transformProps.ts:2:62
prettier/prettier: Delete `␍`
    1 | /**
  > 2 |  * Licensed to the Apache Software Foundation (ASF) under one
      |                                                              ^
    3 |  * or more contributor license agreements.  See the NOTICE file
    4 |  * distributed with this work for additional information
    5 |  * regarding copyright ownership.  The ASF licenses this file

ERROR in src/visualizations/TimeTable/transformProps.ts:3:64
prettier/prettier: Delete `␍`
    1 | /**
    2 |  * Licensed to the Apache Software Foundation (ASF) under one
  > 3 |  * or more contributor license agreements.  See the NOTICE file
      |                                                                ^
    4 |  * distributed with this work for additional information
    5 |  * regarding copyright ownership.  The ASF licenses this file
    6 |  * to you under the Apache License, Version 2.0 (the

ERROR in src/visualizations/TimeTable/transformProps.ts:4:57
prettier/prettier: Delete `␍`
    2 |  * Licensed to the Apache Software Foundation (ASF) under one
    3 |  * or more contributor license agreements.  See the NOTICE file
  > 4 |  * distributed with this work for additional information
      |                                                         ^
    5 |  * regarding copyright ownership.  The ASF licenses this file
    6 |  * to you under the Apache License, Version 2.0 (the
    7 |  * "License"); you may not use this file except in compliance

ERROR in src/visualizations/TimeTable/transformProps.ts:5:62
prettier/prettier: Delete `␍`
    3 |  * or more contributor license agreements.  See the NOTICE file
    4 |  * distributed with this work for additional information
  > 5 |  * regarding copyright ownership.  The ASF licenses this file
      |                                                              ^
    6 |  * to you under the Apache License, Version 2.0 (the
    7 |  * "License"); you may not use this file except in compliance
    8 |  * with the License.  You may obtain a copy of the License at

ERROR in src/visualizations/TimeTable/transformProps.ts:6:53
prettier/prettier: Delete `␍`
    4 |  * distributed with this work for additional information
    5 |  * regarding copyright ownership.  The ASF licenses this file
  > 6 |  * to you under the Apache License, Version 2.0 (the
      |                                                     ^
    7 |  * "License"); you may not use this file except in compliance
    8 |  * with the License.  You may obtain a copy of the License at
    9 |  *

ERROR in src/visualizations/TimeTable/transformProps.ts:7:62
prettier/prettier: Delete `␍`
     5 |  * regarding copyright ownership.  The ASF licenses this file
     6 |  * to you under the Apache License, Version 2.0 (the
  >  7 |  * "License"); you may not use this file except in compliance
       |                                                              ^
     8 |  * with the License.  You may obtain a copy of the License at
     9 |  *
    10 |  *   http://www.apache.org/licenses/LICENSE-2.0

ERROR in src/visualizations/TimeTable/transformProps.ts:8:62
prettier/prettier: Delete `␍`
     6 |  * to you under the Apache License, Version 2.0 (the
     7 |  * "License"); you may not use this file except in compliance
  >  8 |  * with the License.  You may obtain a copy of the License at
       |                                                              ^
     9 |  *
    10 |  *   http://www.apache.org/licenses/LICENSE-2.0
    11 |  *

ERROR in src/visualizations/TimeTable/transformProps.ts:9:3
prettier/prettier: Delete `␍`
     7 |  * "License"); you may not use this file except in compliance
     8 |  * with the License.  You may obtain a copy of the License at
  >  9 |  *
       |   ^
    10 |  *   http://www.apache.org/licenses/LICENSE-2.0
    11 |  *
    12 |  * Unless required by applicable law or agreed to in writing,

ERROR in src/visualizations/TimeTable/transformProps.ts:10:48
prettier/prettier: Delete `␍`
     8 |  * with the License.  You may obtain a copy of the License at
     9 |  *
  > 10 |  *   http://www.apache.org/licenses/LICENSE-2.0
       |                                                ^
    11 |  *
    12 |  * Unless required by applicable law or agreed to in writing,
    13 |  * software distributed under the License is distributed on an

ERROR in src/visualizations/TimeTable/transformProps.ts:11:3
prettier/prettier: Delete `␍`
     9 |  *
    10 |  *   http://www.apache.org/licenses/LICENSE-2.0
  > 11 |  *
       |   ^
    12 |  * Unless required by applicable law or agreed to in writing,
    13 |  * software distributed under the License is distributed on an
    14 |  * "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY

ERROR in src/visualizations/TimeTable/transformProps.ts:12:62
prettier/prettier: Delete `␍`
    10 |  *   http://www.apache.org/licenses/LICENSE-2.0
    11 |  *
  > 12 |  * Unless required by applicable law or agreed to in writing,
       |                                                              ^
    13 |  * software distributed under the License is distributed on an
    14 |  * "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
    15 |  * KIND, either express or implied.  See the License for the

ERROR in src/visualizations/TimeTable/transformProps.ts:13:63
prettier/prettier: Delete `␍`
    11 |  *
    12 |  * Unless required by applicable law or agreed to in writing,
  > 13 |  * software distributed under the License is distributed on an
       |                                                               ^
    14 |  * "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
    15 |  * KIND, either express or implied.  See the License for the
    16 |  * specific language governing permissions and limitations

ERROR in src/visualizations/TimeTable/transformProps.ts:14:58
prettier/prettier: Delete `␍`
    12 |  * Unless required by applicable law or agreed to in writing,
    13 |  * software distributed under the License is distributed on an
  > 14 |  * "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
       |                                                          ^
    15 |  * KIND, either express or implied.  See the License for the
    16 |  * specific language governing permissions and limitations
    17 |  * under the License.

ERROR in src/visualizations/TimeTable/transformProps.ts:15:61
prettier/prettier: Delete `␍`
    13 |  * software distributed under the License is distributed on an
    14 |  * "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
  > 15 |  * KIND, either express or implied.  See the License for the
       |                                                             ^
    16 |  * specific language governing permissions and limitations
    17 |  * under the License.
    18 |  */

ERROR in src/visualizations/TimeTable/transformProps.ts:16:59
prettier/prettier: Delete `␍`
    14 |  * "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
    15 |  * KIND, either express or implied.  See the License for the
  > 16 |  * specific language governing permissions and limitations
       |                                                           ^
    17 |  * under the License.
    18 |  */
    19 | import { ChartProps, DataRecord, Metric } from '@superset-ui/core';

ERROR in src/visualizations/TimeTable/transformProps.ts:17:22
prettier/prettier: Delete `␍`
    15 |  * KIND, either express or implied.  See the License for the
    16 |  * specific language governing permissions and limitations
  > 17 |  * under the License.
       |                      ^
    18 |  */
    19 | import { ChartProps, DataRecord, Metric } from '@superset-ui/core';
    20 |

ERROR in src/visualizations/TimeTable/transformProps.ts:18:4
prettier/prettier: Delete `␍`
    16 |  * specific language governing permissions and limitations
    17 |  * under the License.
  > 18 |  */
       |    ^
    19 | import { ChartProps, DataRecord, Metric } from '@superset-ui/core';
    20 |
    21 | interface FormData {

ERROR in src/visualizations/TimeTable/transformProps.ts:19:68
prettier/prettier: Delete `␍`
    17 |  * under the License.
    18 |  */
  > 19 | import { ChartProps, DataRecord, Metric } from '@superset-ui/core';
       |                                                                    ^
    20 |
    21 | interface FormData {
    22 |   groupby: string[];

ERROR in src/visualizations/TimeTable/transformProps.ts:20:1
prettier/prettier: Delete `␍`
    18 |  */
    19 | import { ChartProps, DataRecord, Metric } from '@superset-ui/core';
  > 20 |
       | ^
    21 | interface FormData {
    22 |   groupby: string[];
    23 |   metrics: Array<object>;

ERROR in src/visualizations/TimeTable/transformProps.ts:21:21
prettier/prettier: Delete `␍`
    19 | import { ChartProps, DataRecord, Metric } from '@superset-ui/core';
    20 |
  > 21 | interface FormData {
       |                     ^
    22 |   groupby: string[];
    23 |   metrics: Array<object>;
    24 |   url: string;

ERROR in src/visualizations/TimeTable/transformProps.ts:22:21
prettier/prettier: Delete `␍`
    20 |
    21 | interface FormData {
  > 22 |   groupby: string[];
       |                     ^
    23 |   metrics: Array<object>;
    24 |   url: string;
    25 |   columnCollection: Array<object> | [];

ERROR in src/visualizations/TimeTable/transformProps.ts:23:26
prettier/prettier: Delete `␍`
    21 | interface FormData {
    22 |   groupby: string[];
  > 23 |   metrics: Array<object>;
       |                          ^
    24 |   url: string;
    25 |   columnCollection: Array<object> | [];
    26 | }

ERROR in src/visualizations/TimeTable/transformProps.ts:24:15
prettier/prettier: Delete `␍`
    22 |   groupby: string[];
    23 |   metrics: Array<object>;
  > 24 |   url: string;
       |               ^
    25 |   columnCollection: Array<object> | [];
    26 | }
    27 |

ERROR in src/visualizations/TimeTable/transformProps.ts:25:40
prettier/prettier: Delete `␍`
    23 |   metrics: Array<object>;
    24 |   url: string;
  > 25 |   columnCollection: Array<object> | [];
       |                                        ^
    26 | }
    27 |
    28 | interface QueryData {

ERROR in src/visualizations/TimeTable/transformProps.ts:26:2
prettier/prettier: Delete `␍`
    24 |   url: string;
    25 |   columnCollection: Array<object> | [];
  > 26 | }
       |  ^
    27 |
    28 | interface QueryData {
    29 |   data: {

ERROR in src/visualizations/TimeTable/transformProps.ts:27:1
prettier/prettier: Delete `␍`
    25 |   columnCollection: Array<object> | [];
    26 | }
  > 27 |
       | ^
    28 | interface QueryData {
    29 |   data: {
    30 |     records: DataRecord[];

ERROR in src/visualizations/TimeTable/transformProps.ts:28:22
prettier/prettier: Delete `␍`
    26 | }
    27 |
  > 28 | interface QueryData {
       |                      ^
    29 |   data: {
    30 |     records: DataRecord[];
    31 |     columns: string[];

ERROR in src/visualizations/TimeTable/transformProps.ts:29:10
prettier/prettier: Delete `␍`
    27 |
    28 | interface QueryData {
  > 29 |   data: {
       |          ^
    30 |     records: DataRecord[];
    31 |     columns: string[];
    32 |   };

ERROR in src/visualizations/TimeTable/transformProps.ts:30:27
prettier/prettier: Delete `␍`
    28 | interface QueryData {
    29 |   data: {
  > 30 |     records: DataRecord[];
       |                           ^
    31 |     columns: string[];
    32 |   };
    33 | }

ERROR in src/visualizations/TimeTable/transformProps.ts:31:23
prettier/prettier: Delete `␍`
    29 |   data: {
    30 |     records: DataRecord[];
  > 31 |     columns: string[];
       |                       ^
    32 |   };
    33 | }
    34 |

ERROR in src/visualizations/TimeTable/transformProps.ts:32:5
prettier/prettier: Delete `␍`
    30 |     records: DataRecord[];
    31 |     columns: string[];
  > 32 |   };
       |     ^
    33 | }
    34 |
    35 | export type TableChartProps = ChartProps & {

ERROR in src/visualizations/TimeTable/transformProps.ts:33:2
prettier/prettier: Delete `␍`
    31 |     columns: string[];
    32 |   };
  > 33 | }
       |  ^
    34 |
    35 | export type TableChartProps = ChartProps & {
    36 |   formData: FormData;

ERROR in src/visualizations/TimeTable/transformProps.ts:34:1
prettier/prettier: Delete `␍`
    32 |   };
    33 | }
  > 34 |
       | ^
    35 | export type TableChartProps = ChartProps & {
    36 |   formData: FormData;
    37 |   queriesData: QueryData[];

ERROR in src/visualizations/TimeTable/transformProps.ts:35:45
prettier/prettier: Delete `␍`
    33 | }
    34 |
  > 35 | export type TableChartProps = ChartProps & {
       |                                             ^
    36 |   formData: FormData;
    37 |   queriesData: QueryData[];
    38 | };

ERROR in src/visualizations/TimeTable/transformProps.ts:36:22
prettier/prettier: Delete `␍`
    34 |
    35 | export type TableChartProps = ChartProps & {
  > 36 |   formData: FormData;
       |                      ^
    37 |   queriesData: QueryData[];
    38 | };
    39 |

ERROR in src/visualizations/TimeTable/transformProps.ts:37:28
prettier/prettier: Delete `␍`
    35 | export type TableChartProps = ChartProps & {
    36 |   formData: FormData;
  > 37 |   queriesData: QueryData[];
       |                            ^
    38 | };
    39 |
    40 | interface ColumnData {

ERROR in src/visualizations/TimeTable/transformProps.ts:38:3
prettier/prettier: Delete `␍`
    36 |   formData: FormData;
    37 |   queriesData: QueryData[];
  > 38 | };
       |   ^
    39 |
    40 | interface ColumnData {
    41 |   timeLag?: string | number;

ERROR in src/visualizations/TimeTable/transformProps.ts:39:1
prettier/prettier: Delete `␍`
    37 |   queriesData: QueryData[];
    38 | };
  > 39 |
       | ^
    40 | interface ColumnData {
    41 |   timeLag?: string | number;
    42 | }

ERROR in src/visualizations/TimeTable/transformProps.ts:40:23
prettier/prettier: Delete `␍`
    38 | };
    39 |
  > 40 | interface ColumnData {
       |                       ^
    41 |   timeLag?: string | number;
    42 | }
    43 | export default function transformProps(chartProps: TableChartProps) {

ERROR in src/visualizations/TimeTable/transformProps.ts:41:29
prettier/prettier: Delete `␍`
    39 |
    40 | interface ColumnData {
  > 41 |   timeLag?: string | number;
       |                             ^
    42 | }
    43 | export default function transformProps(chartProps: TableChartProps) {
    44 |   const { height, datasource, formData, queriesData } = chartProps;

ERROR in src/visualizations/TimeTable/transformProps.ts:42:2
prettier/prettier: Delete `␍`
    40 | interface ColumnData {
    41 |   timeLag?: string | number;
  > 42 | }
       |  ^
    43 | export default function transformProps(chartProps: TableChartProps) {
    44 |   const { height, datasource, formData, queriesData } = chartProps;
    45 |   const { columnCollection = [], groupby, metrics, url } = formData;

ERROR in src/visualizations/TimeTable/transformProps.ts:43:70
prettier/prettier: Delete `␍`
    41 |   timeLag?: string | number;
    42 | }
  > 43 | export default function transformProps(chartProps: TableChartProps) {
       |                                                                      ^
    44 |   const { height, datasource, formData, queriesData } = chartProps;
    45 |   const { columnCollection = [], groupby, metrics, url } = formData;
    46 |   const { records, columns } = queriesData[0].data;

ERROR in src/visualizations/TimeTable/transformProps.ts:44:68
prettier/prettier: Delete `␍`
    42 | }
    43 | export default function transformProps(chartProps: TableChartProps) {
  > 44 |   const { height, datasource, formData, queriesData } = chartProps;
       |                                                                    ^
    45 |   const { columnCollection = [], groupby, metrics, url } = formData;
    46 |   const { records, columns } = queriesData[0].data;
    47 |   const isGroupBy = groupby?.length > 0;

ERROR in src/visualizations/TimeTable/transformProps.ts:45:69
prettier/prettier: Delete `␍`
    43 | export default function transformProps(chartProps: TableChartProps) {
    44 |   const { height, datasource, formData, queriesData } = chartProps;
  > 45 |   const { columnCollection = [], groupby, metrics, url } = formData;
       |                                                                     ^
    46 |   const { records, columns } = queriesData[0].data;
    47 |   const isGroupBy = groupby?.length > 0;
    48 |

ERROR in src/visualizations/TimeTable/transformProps.ts:46:52
prettier/prettier: Delete `␍`
    44 |   const { height, datasource, formData, queriesData } = chartProps;
    45 |   const { columnCollection = [], groupby, metrics, url } = formData;
  > 46 |   const { records, columns } = queriesData[0].data;
       |                                                    ^
    47 |   const isGroupBy = groupby?.length > 0;
    48 |
    49 |   // When there is a "group by",

ERROR in src/visualizations/TimeTable/transformProps.ts:47:41
prettier/prettier: Delete `␍`
    45 |   const { columnCollection = [], groupby, metrics, url } = formData;
    46 |   const { records, columns } = queriesData[0].data;
  > 47 |   const isGroupBy = groupby?.length > 0;
       |                                         ^
    48 |
    49 |   // When there is a "group by",
    50 |   // each row in the table is a database column

ERROR in src/visualizations/TimeTable/transformProps.ts:48:1
prettier/prettier: Delete `␍`
    46 |   const { records, columns } = queriesData[0].data;
    47 |   const isGroupBy = groupby?.length > 0;
  > 48 |
       | ^
    49 |   // When there is a "group by",
    50 |   // each row in the table is a database column
    51 |   // Otherwise each row in the table is a metric

ERROR in src/visualizations/TimeTable/transformProps.ts:49:33
prettier/prettier: Delete `␍`
    47 |   const isGroupBy = groupby?.length > 0;
    48 |
  > 49 |   // When there is a "group by",
       |                                 ^
    50 |   // each row in the table is a database column
    51 |   // Otherwise each row in the table is a metric
    52 |   let rows;

ERROR in src/visualizations/TimeTable/transformProps.ts:50:48
prettier/prettier: Delete `␍`
    48 |
    49 |   // When there is a "group by",
  > 50 |   // each row in the table is a database column
       |                                                ^
    51 |   // Otherwise each row in the table is a metric
    52 |   let rows;
    53 |   if (isGroupBy) {

ERROR in src/visualizations/TimeTable/transformProps.ts:51:49
prettier/prettier: Delete `␍`
    49 |   // When there is a "group by",
    50 |   // each row in the table is a database column
  > 51 |   // Otherwise each row in the table is a metric
       |                                                 ^
    52 |   let rows;
    53 |   if (isGroupBy) {
    54 |     rows = columns.map(column =>

ERROR in src/visualizations/TimeTable/transformProps.ts:52:12
prettier/prettier: Delete `␍`
    50 |   // each row in the table is a database column
    51 |   // Otherwise each row in the table is a metric
  > 52 |   let rows;
       |            ^
    53 |   if (isGroupBy) {
    54 |     rows = columns.map(column =>
    55 |       typeof column === 'object' ? column : { label: column },

ERROR in src/visualizations/TimeTable/transformProps.ts:53:19
prettier/prettier: Delete `␍`
    51 |   // Otherwise each row in the table is a metric
    52 |   let rows;
  > 53 |   if (isGroupBy) {
       |                   ^
    54 |     rows = columns.map(column =>
    55 |       typeof column === 'object' ? column : { label: column },
    56 |     );

ERROR in src/visualizations/TimeTable/transformProps.ts:54:33
prettier/prettier: Delete `␍`
    52 |   let rows;
    53 |   if (isGroupBy) {
  > 54 |     rows = columns.map(column =>
       |                                 ^
    55 |       typeof column === 'object' ? column : { label: column },
    56 |     );
    57 |   } else {

ERROR in src/visualizations/TimeTable/transformProps.ts:55:63
prettier/prettier: Delete `␍`
    53 |   if (isGroupBy) {
    54 |     rows = columns.map(column =>
  > 55 |       typeof column === 'object' ? column : { label: column },
       |                                                               ^
    56 |     );
    57 |   } else {
    58 |     /* eslint-disable */

ERROR in src/visualizations/TimeTable/transformProps.ts:56:7
prettier/prettier: Delete `␍`
    54 |     rows = columns.map(column =>
    55 |       typeof column === 'object' ? column : { label: column },
  > 56 |     );
       |       ^
    57 |   } else {
    58 |     /* eslint-disable */
    59 |     const metricMap = datasource.metrics.reduce(

ERROR in src/visualizations/TimeTable/transformProps.ts:57:11
prettier/prettier: Delete `␍`
    55 |       typeof column === 'object' ? column : { label: column },
    56 |     );
  > 57 |   } else {
       |           ^
    58 |     /* eslint-disable */
    59 |     const metricMap = datasource.metrics.reduce(
    60 |       (acc, current) => {

webpack 5.93.0 compiled with 347887 errors and 426 warnings in 3100889 ms
PS C:\Users\Shivam220802\OneDrive - EXLService.com (I) Pvt. Ltd\Desktop\superset\saa-superset\superset-frontend>
















