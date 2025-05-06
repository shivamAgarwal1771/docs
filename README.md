grade 1a1d627ebd8e -> 55e910a74826, add_metadata_column_to_annotation_model.py
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 55e910a74826 -> 4ce8df208545, empty message
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 4ce8df208545 -> 46f444d8b9b7, remove_coordinator_from_druid_cluster_model.py
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 46f444d8b9b7 -> a61b40f9f57f, remove allow_run_sync    
superset_init  | INFO  [alembic.runtime.migration] Running upgrade a61b40f9f57f -> 6c7537a6004a, models for email reports 
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 6c7537a6004a -> 3e1b21cd94a4, change_owner_to_m2m_relation_on_datasources.py
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 6c7537a6004a -> cefabc8f7d38, Increase size of name column in ab_view_menu
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 55e910a74826 -> 0b1f1ab473c0, Add extra column to Query
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 0b1f1ab473c0, cefabc8f7d38, 3e1b21cd94a4 -> de021a1ca60d, empty message
superset_init  | INFO  [alembic.runtime.migration] Running upgrade de021a1ca60d -> fb13d49b72f9, better_filters
superset_init  | INFO  [alembic.runtime.migration] Running upgrade fb13d49b72f9 -> a33a03f16c4a, Add extra column to SavedQuery
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 4451805bbaa1, 1d9e835a84f9 -> c829ff0b37d0, empty message
superset_init  | INFO  [alembic.runtime.migration] Running upgrade c829ff0b37d0 -> 7467e77870e4, remove_aggs
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 7467e77870e4, de021a1ca60d -> fbd55e0f83eb, empty message
superset_init  | INFO  [alembic.runtime.migration] Running upgrade fbd55e0f83eb, fb13d49b72f9 -> 8b70aa3d0f87, empty message
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 8b70aa3d0f87, a33a03f16c4a -> 18dc26817ad2, empty message
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 18dc26817ad2 -> c617da68de7d, form nullable
superset_init  | INFO  [alembic.runtime.migration] Running upgrade c617da68de7d -> c82ee8a39623, Add implicit tags        
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 18dc26817ad2 -> e553e78e90c5, add_druid_auth_py.py     
superset_init  | INFO  [alembic.runtime.migration] Running upgrade e553e78e90c5, c82ee8a39623 -> 45e7da7cfeba, empty message
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 45e7da7cfeba -> 80aa3f04bc82, Add Parent ids in dashboard layout metadata
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 80aa3f04bc82 -> d94d33dbe938, form strip
superset_init  | INFO  [alembic.runtime.migration] Running upgrade d94d33dbe938 -> 937d04c16b64, update datasources       
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 937d04c16b64 -> 7f2635b51f5d, update base columns      
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 7f2635b51f5d -> e9df189e5c7e, update base metrics      
superset_init  | INFO  [alembic.runtime.migration] Running upgrade e9df189e5c7e -> afc69274c25a, update the sql, select_sql, and executed_sql columns in the
superset_init  |    query table in mysql dbs to be long text columns
superset_init  | INFO  [alembic.runtime.migration] Running upgrade afc69274c25a -> d7c1a0d6f2da, Remove limit used from query model
superset_init  | INFO  [alembic.runtime.migration] Running upgrade d7c1a0d6f2da -> ab8c66efdd01, resample
superset_init  | INFO  [alembic.runtime.migration] Running upgrade ab8c66efdd01 -> b4a38aa87893, deprecate database expression
superset_init  | INFO  [alembic.runtime.migration] Running upgrade b4a38aa87893 -> d6ffdf31bdd4, Add published column to dashboards
superset_init  | INFO  [alembic.runtime.migration] Running upgrade d6ffdf31bdd4 -> 190188938582, Remove duplicated entries in dashboard_slices table and add unique constraint
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 190188938582 -> def97f26fdfb, Add index to tagged_object
superset_init  | INFO  [alembic.runtime.migration] Running upgrade def97f26fdfb -> 11c737c17cc6, deprecate_restricted_metrics
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 11c737c17cc6 -> 258b5280a45e, form strip leading and trailing whitespace
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 258b5280a45e -> 1495eb914ad3, time range
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 1495eb914ad3 -> b6fa807eac07, make_names_non_nullable  
superset_init  | INFO  [alembic.runtime.migration] Running upgrade b6fa807eac07 -> cca2f5d568c8, add encrypted_extra to dbs
superset_init  | INFO  [alembic.runtime.migration] Running upgrade cca2f5d568c8 -> c2acd2cf3df2, alter type of dbs encrypted_extra
superset_init  | INFO  [alembic.runtime.migration] Running upgrade c2acd2cf3df2 -> 78ee127d0d1d, reconvert legacy filters into adhoc
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 78ee127d0d1d -> db4b49eb0782, Add tables for SQL Lab state
superset_init  | INFO  [alembic.runtime.migration] Running upgrade db4b49eb0782 -> 5afa9079866a, serialize_schema_permissions.py
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 5afa9079866a -> 89115a40e8ea, Change table schema description to long text
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 89115a40e8ea -> 817e1c9b09d0, add_not_null_to_dbs_sqlalchemy_url
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 817e1c9b09d0 -> e96dbf2cfef0, datasource_cluster_fk    
superset_init  | INFO  [alembic.runtime.migration] Running upgrade e96dbf2cfef0 -> 3325d4caccc8, empty message
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 3325d4caccc8 -> 0a6f12f60c73, add_role_level_security  
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 0a6f12f60c73 -> 72428d1ea401, Add tmp_schema_name to the query object.
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 72428d1ea401 -> b5998378c225, add certificate to dbs   
superset_init  | INFO  [alembic.runtime.migration] Running upgrade b5998378c225 -> f9a30386bd74, cleanup_time_granularity 
superset_init  | INFO  [alembic.runtime.migration] Running upgrade f9a30386bd74 -> 620241d1153f, update time_grain_sqla   
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 620241d1153f -> 743a117f0d98, Add slack to the schedule
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 743a117f0d98 -> e557699a813e, add_tables_relation_to_row_level_security
superset_init  | INFO  [alembic.runtime.migration] Running upgrade e557699a813e -> ea396d202291, Add ctas_method to the Query object
superset_init  | INFO  [alembic.runtime.migration] Running upgrade ea396d202291 -> a72cb0ebeb22, deprecate dbs.perm column
superset_init  | INFO  [alembic.runtime.migration] Running upgrade a72cb0ebeb22 -> 2f1d15e8a6af, add_alerts
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 2f1d15e8a6af -> f2672aa8350a, add_slack_to_alerts      
superset_init  | INFO  [alembic.runtime.migration] Running upgrade f2672aa8350a -> f120347acb39, Add extra column to tables and metrics
superset_init  | INFO  [alembic.runtime.migration] Running upgrade f2672aa8350a -> 978245563a02, Migrate iframe in dashboard to markdown component
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 978245563a02, f120347acb39 -> f80a3b88324b, empty message
superset_init  | INFO  [alembic.runtime.migration] Running upgrade f80a3b88324b -> 2e5a0ee25ed4, refractor_alerting       
superset_init  | INFO  [alembic.runtime.migration] Running upgrade f80a3b88324b -> 175ea3592453, Add cache to datasource lookup table.
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 175ea3592453, 2e5a0ee25ed4 -> ae19b4ee3692, empty message
superset_init  | INFO  [alembic.runtime.migration] Running upgrade ae19b4ee3692 -> e5ef6828ac4e, add rls filter type and grouping key
superset_init  | INFO  [alembic.runtime.migration] Running upgrade e5ef6828ac4e -> 3fbbc6e8d654, fix data access permissions for virtual datasets
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 3fbbc6e8d654 -> 18532d70ab98, Delete table_name unique constraint in mysql
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 18532d70ab98 -> b56500de1855, add_uuid_column_to_import_mixin
superset_init  |
superset_init  | Cleaning up slice uuid from dashboard positiCleaning up slice uuid from dashboard position json.. Done.  

superset_init  |
superset_init  | INFO  [alembic.runtime.migration] Running upgrade b56500de1855 -> af30ca79208f, Collapse alerting models into a single one
superset_init  | INFO  [alembic.runtime.migration] Running upgrade af30ca79208f -> 585b0b1a7b18, add exec info to saved queries
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 585b0b1a7b18 -> 96e99fb176a0, add_import_mixing_to_saved_query
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 96e99fb176a0 -> 49b5a32daba5, add report schedules     
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 49b5a32daba5 -> a8173232b786, Add path to logs
superset_init  | INFO  [alembic.runtime.migration] Running upgrade a8173232b786 -> e38177dbf641, security converge saved queries
superset_init  | INFO  [alembic.runtime.migration] Running upgrade e38177dbf641 -> 8ee129739cf9, security converge css templates
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 8ee129739cf9 -> 811494c0cc23, Remove path, path_no_int, and ref from logs
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 811494c0cc23 -> 5daced1f0e76, reports add working_timeout column
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 5daced1f0e76 -> 40f16acf1ba7, security converge reports
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 40f16acf1ba7 -> ccb74baaa89b, security converge charts 
superset_init  | INFO  [alembic.runtime.migration] Running upgrade ccb74baaa89b -> c25cb2c78727, security converge annotations
superset_init  | INFO  [alembic.runtime.migration] Running upgrade c25cb2c78727 -> 45731db65d9c, security converge datasets
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 45731db65d9c -> 4b84f97828aa, security converge logs   
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 4b84f97828aa -> 1f6dca87d1a2, security converge dashboards
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 1f6dca87d1a2 -> 42b4c9e01447, security converge databases
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 42b4c9e01447 -> e37912a26567, security converge queries
superset_init  | INFO  [alembic.runtime.migration] Running upgrade e37912a26567 -> ab104a954a8f, reports alter crontab size
superset_init  | INFO  [alembic.runtime.migration] Running upgrade ab104a954a8f -> 73fd22e742ab, add_dynamic_plugins.py   
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 73fd22e742ab -> c878781977c6, alert reports shared uniqueness
superset_init  | INFO  [alembic.runtime.migration] Running upgrade c878781977c6 -> 260bf0649a77, migrate [x dateunit] to [x dateunit ago/later]
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 260bf0649a77 -> e11ccdd12658, add roles relationship to dashboard
superset_init  | INFO  [alembic.runtime.migration] Running upgrade e11ccdd12658 -> 41ce8799acc3, rename pie label type    
superset_init  | Updated 0 pie chart labels.
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 41ce8799acc3 -> 070c043f2fdb, add granularity to charts where missing
superset_init  | 0 slices altered
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 070c043f2fdb -> c501b7c653a3, add missing uuid column  
superset_init  |
superset_init  | Cleaning up slice uuid from dashboard positiCleaning up slice uuid from dashboard position json.. Done.  

superset_init  |
superset_init  | INFO  [alembic.runtime.migration] Running upgrade c501b7c653a3 -> 1412ec1e5a7b, legacy force directed to echart
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 1412ec1e5a7b -> 67da9ef1ef9c, add hide_left_bar to tabstate
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 67da9ef1ef9c -> 989bbe479899, rename_filter_configuration_in_dashboard_metadata.py
superset_init  | Updated 0 native filter configurations.     
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 989bbe479899 -> 301362411006, add_execution_id_to_report_execution_log_model.py
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 301362411006 -> 134cea61c5e7, remove dataset health check message
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 134cea61c5e7 -> 085f06488938, Country map use lowercase country name
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 085f06488938 -> fc3a3a8ff221, migrate filter sets to new format
superset_init  | Updated 0 filter sets with 0 filters.       
superset_init  | INFO  [alembic.runtime.migration] Running upgrade fc3a3a8ff221 -> 19e978e1b9c3, add_report_format_to_report_schedule_model.py
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 19e978e1b9c3 -> d416d0d715cc, add_limiting_factor_column_to_query_model.py
superset_init  | INFO  [alembic.runtime.migration] Running upgrade d416d0d715cc -> f1410ed7ec95, migrate native filters to new schema
superset_init  | Upgraded 0 filters and 0 filter sets.       
superset_init  | INFO  [alembic.runtime.migration] Running upgrade f1410ed7ec95 -> 453530256cea, add_save_form_column_to_db_model
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 453530256cea -> 3317e9248280, add_creation_method_to_reports_model
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 3317e9248280 -> 030c840e3a1c, Add query context to slices
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 030c840e3a1c -> ae1ed299413b, add_timezone_to_report_schedule
superset_init  | INFO  [alembic.runtime.migration] Running upgrade ae1ed299413b -> 31b2a1039d4a, drop tables constraint   
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 31b2a1039d4a -> e323605f370a, fix schemas_allowed_for_csv_upload
superset_init  | INFO  [alembic.runtime.migration] Running upgrade e323605f370a -> 143b6f2815da, migrate pivot table v2 heatmaps to new format
superset_init  | Upgraded 0 slices.
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 143b6f2815da -> f6196627326f, update chart permissions 
superset_init  | INFO  [alembic.runtime.migration] Running upgrade f6196627326f -> 6d20ba9ecb33, add_last_saved_at_to_slice_model
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 6d20ba9ecb33 -> 07071313dd52, change_fetch_values_predicate_to_text
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 07071313dd52 -> 021b81fe4fbb, Add type to native filter configuration
superset_init  | INFO  [alembic] [AddTypeToNativeFilter] Starting upgrade
superset_init  | INFO  [alembic] [AddTypeToNativeFilter] Done!
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 021b81fe4fbb -> 181091c0ef16, add_extra_column_to_columns_model
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 181091c0ef16 -> 3ebe0993c770, add filter set model     
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 3ebe0993c770 -> 60dc453f4e2e, migrate timeseries_limit_metric to legacy_order_by in pivot_table_v2
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 60dc453f4e2e -> 32646df09c64, update time grain SQLA   
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 32646df09c64 -> f9847149153d, add_certifications_columns_to_slice
superset_init  | INFO  [alembic.runtime.migration] Running upgrade f9847149153d -> aea15018d53b, add_certifications_columns_to_dashboard
superset_init  | INFO  [alembic.runtime.migration] Running upgrade aea15018d53b -> b92d69a6643c, rename_csv_to_file       
superset_init  | INFO  [alembic.runtime.migration] Running upgrade b92d69a6643c -> 0ca9e5f1dacd, rename to schemas_allowed_for_file_upload in dbs.extra
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 0ca9e5f1dacd -> abe27eaf93db, add_extra_config_column_to_alerts
superset_init  | INFO  [alembic.runtime.migration] Running upgrade abe27eaf93db -> 3ba29ecbaac5, Change datatype of type in BaseColumn
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 3ba29ecbaac5 -> fe23025b9441, rename_big_viz_total_form_data_fields
superset_init  | INFO  [alembic.runtime.migration] Running upgrade fe23025b9441 -> 31bb738bd1d2, move_pivot_table_v2_legacy_order_by_to_timeseries_limit_metric
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 31bb738bd1d2 -> bb38f40aa3ff, Add force_screenshot to alerts/reports
superset_init  | INFO  [alembic.runtime.migration] Running upgrade bb38f40aa3ff -> c53bae8f08dd, add_saved_query_foreign_key_to_tab_state
superset_init  | INFO  [alembic.runtime.migration] Running upgrade c53bae8f08dd -> 5fd49410a97a, Add columns for external management
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 5fd49410a97a -> 5afbb1a5849b, add_embedded_dashboard_table
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 5afbb1a5849b -> b8d3a24d9131, New dataset models       
superset_init  | INFO  [alembic.runtime.migration] Running upgrade b8d3a24d9131 -> b5a422d8e252, fix query and saved_query null schema
superset_init  | INFO  [alembic.runtime.migration] Running upgrade b5a422d8e252 -> ab9a9d86e695, deprecate time_range_endpoints
superset_init  | INFO  [alembic.runtime.migration] Running upgrade ab9a9d86e695 -> 7293b0ca7944, change_adhoc_filter_b_from_none_to_empty_array
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 7293b0ca7944 -> 6766938c6065, add key-value store      
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 6766938c6065 -> 58df9d617f14, add_on_saved_query_delete_tab_state_null_constraint"
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 58df9d617f14 -> 2ed890b36b94, rm_time_range_endpoints_from_qc
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 2ed890b36b94 -> b0d0249074e4, deprecate time_range_endpoints v2
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 2ed890b36b94 -> 8b841273bec3, sql_lab_models_database_constraint_updates
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 8b841273bec3, b0d0249074e4 -> 9d8a8d575284, merge point
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 9d8a8d575284 -> cecc6bf46990, rm_time_range_endpoints_2
superset_init  | INFO  [alembic.runtime.migration] Running upgrade cecc6bf46990 -> ad07e4fdbaba, rm_time_range_endpoints_from_qc_3
superset_init  | slices updated with no time_range_endpoints: 0
superset_init  | INFO  [alembic.runtime.migration] Running upgrade ad07e4fdbaba -> a9422eeaae74, new_dataset_models_take_2
superset_init  | >> Assign new UUIDs to tables...
superset_init  | >> Drop intermediate columns...
superset_init  | INFO  [alembic.runtime.migration] Running upgrade a9422eeaae74 -> cbe71abde154, fix report schedule and execution log
superset_init  | INFO  [alembic.runtime.migration] Running upgrade cbe71abde154 -> 6f139c533bea, adding advanced data type to column models
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 6f139c533bea -> e786798587de, Delete None permissions  
superset_init  | INFO  [alembic.runtime.migration] Running upgrade e786798587de -> e09b4ae78457, Resize key_value blob    
superset_init  | INFO  [alembic.runtime.migration] Running upgrade e09b4ae78457 -> f3afaf1f11f0, add_unique_name_desc_rls 
superset_init  | INFO  [alembic.runtime.migration] Running upgrade f3afaf1f11f0 -> 7fb8bca906d2, permalink_rename_filterState
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 7fb8bca906d2 -> cdcf3d64daf4, Add user_id and dttm composite index to Log model
superset_init  | INFO  [alembic.runtime.migration] Running upgrade cdcf3d64daf4 -> c747c78868b6, Migrating legacy TreeMap 
superset_init  | INFO  [alembic.runtime.migration] Running upgrade c747c78868b6 -> 06e1e70058c7, Migrating legacy Area    
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 06e1e70058c7 -> a39867932713, query_context_to_mediumtext
superset_init  | INFO  [alembic.runtime.migration] Running upgrade a39867932713 -> 409c7b420ab0, add created_by_fk as owner
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 409c7b420ab0 -> ffa79af61a56, rename report_schedule.extra to extra_json
superset_init  | INFO  [alembic.runtime.migration] Running upgrade ffa79af61a56 -> 6d3c6f9d665d, fix_table_chart_conditional_formatting_colors
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 6d3c6f9d665d -> 291f024254b5, drop_column_allow_multi_schema_metadata_fetch
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 291f024254b5 -> deb4c9d4a4ef, parameters in saved queries
superset_init  | INFO  [alembic.runtime.migration] Running upgrade deb4c9d4a4ef -> 4ce1d9b25135, remove_filter_bar_orientation
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 4ce1d9b25135 -> f3c2d8ec8595, create_ssh_tunnel_credentials_tbl
superset_init  | INFO  [alembic.runtime.migration] Running upgrade f3c2d8ec8595 -> 9c2a5681ddfd, convert key-value entries to json
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 9c2a5681ddfd -> c0a3ea245b61, remove_show_native_filters
superset_init  | INFO  [alembic.runtime.migration] Running upgrade c0a3ea245b61 -> d0ac08bb5b83, invert_horizontal_bar_chart_order
superset_init  | INFO  [alembic.runtime.migration] Running upgrade d0ac08bb5b83 -> b5ea9d343307, bar_chart_stack_options  
superset_init  | INFO  [alembic.runtime.migration] Running upgrade b5ea9d343307 -> 07f9a902af1b, drop postgres enum constrains for tags
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 07f9a902af1b -> 7e67aecbf3f1, chart-ds-constraint      
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 7e67aecbf3f1 -> 4ea966691069, cross-filter-global-scoping
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 4ea966691069 -> 9ba2ce3086e5, migrate-pivot-table-v1-to-v2
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 9ba2ce3086e5 -> 4c5da39be729, migrate_treemap_chart    
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 4c5da39be729 -> ae58e1e58e5c, migrate_dual_line_to_mixed_chart
superset_init  | INFO  [alembic.runtime.migration] Running upgrade ae58e1e58e5c -> 83e1abbe777f, drop access_request      
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 83e1abbe777f -> 90139bf715e4, add_currency_column_to_metrics
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 90139bf715e4 -> 6fbe660cac39, add on delete cascade for tables references
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 6fbe660cac39 -> 8e5b0fb85b9a, Add custom size columns to report schedule
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 8e5b0fb85b9a -> 240d23c7f86f, update_tag_model_w_description
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 240d23c7f86f -> f92a3124dd66, drop rouge constraints and tables
superset_init  | INFO  [alembic.runtime.migration] Running upgrade f92a3124dd66 -> 6d05b0a70c89, add on delete cascade for owners references
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 6d05b0a70c89 -> 863adcf72773, delete obsolete Druid NoSQL slice parameters
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 863adcf72773 -> a23c6f8b1280, cleanup erroneous parent filter IDs
superset_init  | INFO  [alembic.runtime.migration] Running upgrade a23c6f8b1280 -> bf646a0c1501, json_metadata
superset_init  | INFO  [alembic.runtime.migration] Running upgrade bf646a0c1501 -> e0f6f91c2055, create_user_favorite_table
superset_init  | INFO  [alembic.runtime.migration] Running upgrade e0f6f91c2055 -> ee179a490af9, deckgl-path-width-units  
superset_init  | INFO  [alembic.runtime.migration] Running upgrade ee179a490af9 -> 0769ef90fddd, Fix schema perm for datasets
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 0769ef90fddd -> 2e826adca42c, Fix schema for log       
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 2e826adca42c -> 8ace289026f3, add on delete cascade for dashboard_slices
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 8ace289026f3 -> 4448fa6deeb1, add on delete cascade for embedded_dashboards
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 4448fa6deeb1 -> 9f4a086c2676, add_normalize_columns_to_sqla_model
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 9f4a086c2676 -> ec54aca4c8a2, Increase ab_user.email field size
superset_init  | INFO  [alembic.runtime.migration] Running upgrade ec54aca4c8a2 -> 317970b4400c, Added always_filter_main_dttm to datasource
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 317970b4400c -> 4b85906e5b91, add on delete cascade for dashboard_roles
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 4b85906e5b91 -> b7851ee5522f, replay 317970b4400c      
superset_init  | INFO  [alembic.runtime.migration] Running upgrade b7851ee5522f -> 06dd9ff00fe8, add_percent_calculation_type_funnel_chart
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 06dd9ff00fe8 -> 65a167d4c62e, add indexes to report models
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 65a167d4c62e -> 59a1450b3c10, drop_filter_sets_table   
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 59a1450b3c10 -> a32e0c4d8646, migrate-sunburst-chart   
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 59a1450b3c10 -> 96164e3017c6
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 96164e3017c6, a32e0c4d8646 -> 15a2c68a2e6b, merging two heads
superset_init  | INFO  [alembic.runtime.migration] Running upgrade a32e0c4d8646 -> 214f580d09c9, migrate_filter_boxes_to_native_filters
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 214f580d09c9 -> e863403c0c50, drop_url_table
superset_init  | INFO  [alembic.runtime.migration] Running upgrade e863403c0c50, 15a2c68a2e6b -> 1cf8e4344e2b, merging    
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 1cf8e4344e2b -> 87d38ad83218, Migrate can_view_and_drill permission
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 87d38ad83218 -> 17fcea065655, change_text_to_mediumtext
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 17fcea065655 -> be1b217cd8cd, big_number_kpi_single_metric
superset_init  | INFO  [alembic.runtime.migration] Running upgrade be1b217cd8cd -> 678eefb4ab44, Add access token table   
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 678eefb4ab44 -> c22cb5c2e546, empty message
superset_init  | Adding column 'avatar_url' to table 'user_attribute'.
superset_init  |
superset_init  | INFO  [alembic.runtime.migration] Running upgrade c22cb5c2e546 -> 5ad7321c2169, mig new csv upload perm  
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 5ad7321c2169 -> d60591c5515f, mig new excel upload perm
superset_init  | INFO  [alembic.runtime.migration] Running upgrade d60591c5515f -> 5f57af97bc3f, Add catalog column       
superset_init  | Adding column 'catalog' to table 'tables'.  
superset_init  |
superset_init  | Adding column 'catalog' to table 'query'.   
superset_init  |
superset_init  | Adding column 'catalog' to table 'saved_query'.
superset_init  |
superset_init  | Adding column 'catalog' to table 'tab_state'.
superset_init  |
superset_init  | Adding column 'catalog' to table 'table_schema'.
superset_init  |
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 5f57af97bc3f -> 3dfd0e78650e, add_query_sql_editor_id_index
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 5f57af97bc3f -> 4a33124c18ad, mig new columnar upload perm
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 4a33124c18ad -> 58d051681a3b, Add catalog_perm to tables
superset_init  | Adding column 'catalog_perm' to table 'tables'.
superset_init  |
superset_init  | Adding column 'catalog_perm' to table 'slices'.
superset_init  |
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 58d051681a3b, 3dfd0e78650e -> 645bb206f96c, empty message
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 645bb206f96c -> 4081be5b6b74, Enable catalog in Databricks
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 4081be5b6b74 -> 87ffc36f9842, Enable catalog in BigQuery/Presto/Trino/Snowflake
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 87ffc36f9842 -> 9621c6d56ffb, add subject column to report schedule
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 9621c6d56ffb -> f84fde59123a, Update charts with old time comparison controls
superset_init  | INFO  [alembic.runtime.migration] Running upgrade f84fde59123a -> f7b6750b67e8, change_mediumtext_to_longtext
superset_init  | Revision ID: f7b6750b67e8
superset_init  | Revises: f84fde59123a
superset_init  | Create Date: 2024-05-09 19:19:46.630140     
superset_init  | INFO  [alembic.runtime.migration] Running upgrade f7b6750b67e8 -> 02f4f7811799, remove sl_dataset_columns tables
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 02f4f7811799 -> 39549add7bfc, remove sl_table_columns_table
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 39549add7bfc -> 38f4144e8558, remove sl_dataset_tables 
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 38f4144e8558 -> e53fd48cc078, remove sl_dataset_users  
superset_init  | INFO  [alembic.runtime.migration] Running upgrade e53fd48cc078 -> a6b32d2d07b1, remove sl_columns        
superset_init  | INFO  [alembic.runtime.migration] Running upgrade a6b32d2d07b1 -> 007a1abffe7e, remove sl_tables
superset_init  | INFO  [alembic.runtime.migration] Running upgrade 007a1abffe7e -> 48cbb571fa3a, remove sl_datasets       
superset_init  | INFO  [alembic.env] Migration scripts completed. Duration: 00:00:13
superset_init  | ######################################################################
superset_init  | Init Step 1/4 [Complete] -- Applying DB migrations
superset_init  | ######################################################################
superset_init  | ######################################################################
superset_init  | Init Step 2/4 [Starting] -- Setting up admin user ( admin / admin )
superset_init  | ######################################################################
superset_init  | Loaded your LOCAL configuration at [/app/docker/pythonpath_dev/superset_config.py]
superset_init  | 2025-05-06 08:25:29,742:INFO:superset.initialization:Setting database isolation level to READ COMMITTED  
superset_init  | Recognized Database Authentications.        
superset_init  | Admin User admin created.
superset_init  | ######################################################################
superset_init  | Init Step 2/4 [Complete] -- Setting up admin user
superset_init  | ######################################################################
superset_init  | ######################################################################
superset_init  | Init Step 3/4 [Starting] -- Setting up roles and perms
superset_init  | ######################################################################
superset_init  | Loaded your LOCAL configuration at [/app/docker/pythonpath_dev/superset_config.py]
superset_init  | 2025-05-06 08:25:35,938:INFO:superset.initialization:Setting database isolation level to READ COMMITTED  
superset_init  | 2025-05-06 08:25:46,270:INFO:superset.security.manager:Syncing role definition
superset_init  | 2025-05-06 08:25:46,539:INFO:superset.security.manager:Syncing Admin perms
superset_init  | 2025-05-06 08:25:46,546:INFO:superset.security.manager:Syncing Alpha perms
superset_init  | 2025-05-06 08:25:46,928:INFO:superset.security.manager:Syncing Gamma perms
superset_init  | 2025-05-06 08:25:47,321:INFO:superset.security.manager:Syncing sql_lab perms
superset_init  | 2025-05-06 08:25:47,676:INFO:superset.security.manager:Fetching a set of all perms to lookup which ones are missing
superset_init  | 2025-05-06 08:25:47,688:INFO:superset.security.manager:Creating missing datasource permissions.
superset_init  | 2025-05-06 08:25:47,697:INFO:superset.security.manager:Creating missing database permissions.
superset_init  | 2025-05-06 08:25:47,705:INFO:superset.security.manager:Cleaning faulty perms
superset_init  | ######################################################################
superset_init  | Init Step 3/4 [Complete] -- Setting up roles and perms
superset_init  | ######################################################################
superset_init  | ######################################################################
superset_init  | Init Step 4/4 [Starting] -- Loading examples
superset_init  | ######################################################################
superset_init  | Loaded your LOCAL configuration at [/app/docker/pythonpath_dev/superset_config.py]
superset_init  | 2025-05-06 08:25:52,148:INFO:superset.initialization:Setting database isolation level to READ COMMITTED  
superset_init  | 2025-05-06 08:25:53,683:INFO:superset.utils.database:Creating database reference for examples
superset_init  | Loading examples metadata and related data into examples
superset_init  | Creating default CSS templates
superset_init  | Loading [World Bank's Health Nutrition and Population Stats]
superset_init  | Creating table [wb_health_population] reference
superset_init  | Creating a World's Health Bank dashboard    
superset_init  | Loading [Birth names]
superset_init  | Done loading table!
superset_init  | --------------------------------------------------------------------------------
superset_init  | Creating table [birth_names] reference      
superset_init  | Creating some slices
superset_init  | Creating a dashboard
superset_init  | Loading [Random long/lat data]
superset_init  | Done loading table!
superset_init  | --------------------------------------------------------------------------------
superset_init  | Creating table reference
superset_init  | Creating a slice
superset_init  | Loading [Country Map data]
superset_init  | Done loading table!
superset_init  | --------------------------------------------------------------------------------
superset_init  | Creating table reference
superset_init  | Creating a slice
superset_init  | Loading [San Francisco population polygons] 
superset_init  | Creating table sf_population_polygons reference
superset_init  | Loading [Flights data]
superset_init  | Done loading table!
superset_init  | Loading [BART lines]
superset_init  | Traceback (most recent call last):
superset_init  |   File "/usr/local/bin/superset", line 8, in <module>
superset_init  |     sys.exit(superset())
superset_init  |   File "/usr/local/lib/python3.10/site-packages/click/core.py", line 1157, in __call__
superset_init  |     return self.main(*args, **kwargs)       
superset_init  |   File "/usr/local/lib/python3.10/site-packages/click/core.py", line 1078, in main
superset_init  |     rv = self.invoke(ctx)
superset_init  |   File "/usr/local/lib/python3.10/site-packages/click/core.py", line 1688, in invoke
superset_init  |     return _process_result(sub_ctx.command.invoke(sub_ctx))
superset_init  |   File "/usr/local/lib/python3.10/site-packages/click/core.py", line 1434, in invoke
superset_init  |     return ctx.invoke(self.callback, **ctx.params)
superset_init  |   File "/usr/local/lib/python3.10/site-packages/click/core.py", line 783, in invoke
superset_init  |     return __callback(*args, **kwargs)      
superset_init  |   File "/usr/local/lib/python3.10/site-packages/click/decorators.py", line 33, in new_func
superset_init  |     return f(get_current_context(), *args, **kwargs)
superset_init  |   File "/usr/local/lib/python3.10/site-packages/flask/cli.py", line 358, in decorator
superset_init  |     return __ctx.invoke(f, *args, **kwargs) 
superset_init  |   File "/usr/local/lib/python3.10/site-packages/click/core.py", line 783, in invoke
superset_init  |     return __callback(*args, **kwargs)      
superset_init  |   File "/app/superset/utils/decorators.py", line 266, in wrapped
superset_init  |     return on_error(ex)
superset_init  |   File "/app/superset/utils/decorators.py", line 236, in on_error
superset_init  |     raise ex
superset_init  |   File "/app/superset/utils/decorators.py", line 259, in wrapped
superset_init  |     result = func(*args, **kwargs)
superset_init  |   File "/app/superset/cli/examples.py", line 115, in load_examples
superset_init  |     load_examples_run(load_test_data, load_big_data, only_metadata, force)
superset_init  |   File "/app/superset/cli/examples.py", line 75, in load_examples_run
superset_init  |     examples.load_bart_lines(only_metadata, force)
superset_init  |   File "/app/superset/examples/bart_lines.py", line 39, in load_bart_lines
superset_init  |     df = pd.read_json(url, encoding="latin-1", compression="gzip")
superset_init  |   File "/usr/local/lib/python3.10/site-packages/pandas/io/json/_json.py", line 760, in read_json
superset_init  |     json_reader = JsonReader(
superset_init  |   File "/usr/local/lib/python3.10/site-packages/pandas/io/json/_json.py", line 861, in __init__
superset_init  |     data = self._get_data_from_filepath(filepath_or_buffer)
superset_init  |   File "/usr/local/lib/python3.10/site-packages/pandas/io/json/_json.py", line 901, in _get_data_from_filepath
superset_init  |     self.handles = get_handle(
superset_init  |   File "/usr/local/lib/python3.10/site-packages/pandas/io/common.py", line 716, in get_handle
superset_init  |     ioargs = _get_filepath_or_buffer(       
superset_init  |   File "/usr/local/lib/python3.10/site-packages/pandas/io/common.py", line 368, in _get_filepath_or_buffer
superset_init  |     with urlopen(req_info) as req:
superset_init  |   File "/usr/local/lib/python3.10/site-packages/pandas/io/common.py", line 270, in urlopen
superset_init  |     return urllib.request.urlopen(*args, **kwargs)
superset_init  |   File "/usr/local/lib/python3.10/urllib/request.py", line 216, in urlopen
superset_init  |     return opener.open(url, data, timeout)  
superset_init  |   File "/usr/local/lib/python3.10/urllib/request.py", line 525, in open
superset_init  |     response = meth(req, response)
superset_init  |   File "/usr/local/lib/python3.10/urllib/request.py", line 634, in http_response
superset_init  |     response = self.parent.error(
superset_init  |   File "/usr/local/lib/python3.10/urllib/request.py", line 563, in error
superset_init  |     return self._call_chain(*args)
superset_init  |   File "/usr/local/lib/python3.10/urllib/request.py", line 496, in _call_chain
superset_init  |     result = func(*args)
superset_init  |   File "/usr/local/lib/python3.10/urllib/request.py", line 643, in http_error_default
superset_init  |     raise HTTPError(req.full_url, code, msg, hdrs, fp)
superset_init  | urllib.error.HTTPError: HTTP Error 429: Too Many Requests
