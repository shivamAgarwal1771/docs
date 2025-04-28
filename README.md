2025-04-28 18:35:07 superset_app          | Skipping local overrides
2025-04-28 18:35:07 superset_app          | Starting web app...
2025-04-28 18:35:07 superset_app          | [2025-04-28 13:05:07 +0000] [8] [INFO] Starting gunicorn 23.0.0
2025-04-28 18:35:07 superset_app          | [2025-04-28 13:05:07 +0000] [8] [INFO] Listening at: http://0.0.0.0:8088 (8)
2025-04-28 18:35:07 superset_app          | [2025-04-28 13:05:07 +0000] [8] [INFO] Using worker: gthread
2025-04-28 18:35:07 superset_app          | [2025-04-28 13:05:07 +0000] [9] [INFO] Booting worker with pid: 9
2025-04-28 18:35:05 superset_worker_beat  | Reinstalling the app in editable mode
2025-04-28 18:35:08 superset_worker_beat  | Resolved 147 packages in 2.32s
2025-04-28 18:34:18 superset_init         | Reinstalling the app in editable mode
2025-04-28 18:34:19 superset_init         | Resolved 147 packages in 1.56s
2025-04-28 18:34:19 superset_init         |    Building apache-superset @ file:///app
2025-04-28 18:34:21 superset_init         |       Built apache-superset @ file:///app
2025-04-28 18:34:21 superset_init         | Prepared 1 package in 1.49s
2025-04-28 18:34:21 superset_init         | Uninstalled 1 package in 103ms
2025-04-28 18:34:21 superset_init         | warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
2025-04-28 18:34:21 superset_init         |          If the cache and target directories are on different filesystems, hardlinking may not be supported.
2025-04-28 18:34:21 superset_init         |          If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
2025-04-28 18:34:21 superset_init         | Installed 1 package in 0.93ms
2025-04-28 18:34:21 superset_init         |  ~ apache-superset==0.0.0.dev0 (from file:///app)
2025-04-28 18:34:21 superset_init         | Installing postgres requirements
2025-04-28 18:34:22 superset_init         | Resolved 148 packages in 1.38s
2025-04-28 18:34:22 superset_init         |    Building apache-superset @ file:///app
2025-04-28 18:34:24 superset_init         |       Built apache-superset @ file:///app
2025-04-28 18:34:24 superset_init         | Prepared 1 package in 1.43s
2025-04-28 18:34:24 superset_init         | Uninstalled 1 package in 0.39ms
2025-04-28 18:34:24 superset_init         | warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
2025-04-28 18:34:24 superset_init         |          If the cache and target directories are on different filesystems, hardlinking may not be supported.
2025-04-28 18:34:24 superset_init         |          If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
2025-04-28 18:34:24 superset_init         | Installed 1 package in 1ms
2025-04-28 18:34:24 superset_init         |  ~ apache-superset==0.0.0.dev0 (from file:///app)
2025-04-28 18:34:24 superset_init         | Skipping local overrides
2025-04-28 18:34:24 superset_init         | Unknown Operation!!!
2025-04-28 18:34:24 superset_init         | ######################################################################
2025-04-28 18:34:24 superset_init         | Init Step 1/4 [Starting] -- Applying DB migrations
2025-04-28 18:34:24 superset_init         | ######################################################################
2025-04-28 18:34:29 superset_init         | Loaded your LOCAL configuration at [/app/docker/pythonpath_dev/superset_config.py]
2025-04-28 18:34:29 superset_init         | 2025-04-28 13:04:29,724:INFO:superset.initialization:Setting database isolation level to READ COMMITTED
2025-04-28 18:34:30 superset_init         | 2025-04-28 13:04:30,032:INFO:superset.utils.screenshots:No PIL installation found
2025-04-28 18:34:30 superset_init         | 2025-04-28 13:04:30,479:INFO:superset.utils.pdf:No PIL installation found
2025-04-28 18:34:31 superset_init         | INFO  [alembic.env] Starting the migration scripts.
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Will assume transactional DDL.
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade  -> 4e6a06bad7a8, Init
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Creating table clusters...
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Table clusters created.
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Creating table dashboards...
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Table dashboards created.
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Creating table dbs...
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Table dbs created.
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Creating table datasources...
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Table datasources created.
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Creating table tables...
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Table tables created.
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Creating table columns...
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Table columns created.
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Creating table metrics...
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Table metrics created.
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Creating table slices...
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Table slices created.
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Creating table sql_metrics...
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Table sql_metrics created.
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Creating table table_columns...
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Table table_columns created.
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Creating table dashboard_slices...
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Table dashboard_slices created.
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 4e6a06bad7a8 -> 5a7bad26f2a7, empty message
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 5a7bad26f2a7 -> 1e2841a4128, empty message
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 1e2841a4128 -> 2929af7925ed, TZ offsets in data sources
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 2929af7925ed -> 289ce07647b, Add encrypted password field
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 289ce07647b -> 1a48a5411020, adding slug to dash
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 1a48a5411020 -> 315b3f4da9b0, adding log model
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Creating table logs...
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Table logs created.
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 315b3f4da9b0 -> 55179c7f25c7, sqla_descr
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 55179c7f25c7 -> 12d55656cbca, is_featured
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 12d55656cbca -> 2591d77e9831, user_id
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 2591d77e9831 -> 8e80a26a31db, empty message
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Creating table url...
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Table url created.
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 8e80a26a31db -> 7dbf98566af7, empty message
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 7dbf98566af7 -> 43df8de3a5f4, empty message
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 43df8de3a5f4 -> d827694c7555, css templates
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Creating table css_templates...
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Table css_templates created.
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade d827694c7555 -> 430039611635, log more
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 430039611635 -> 18e88e1cc004, making audit nullable
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 18e88e1cc004 -> 836c0bf75904, cache_timeouts
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 18e88e1cc004 -> a2d606a761d9, adding favstar model
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Creating table favstar...
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Table favstar created.
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade a2d606a761d9, 836c0bf75904 -> d2424a248d63, empty message
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade d2424a248d63 -> 763d4b211ec9, fixing audit fk
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade d2424a248d63 -> 1d2ddd543133, log dt
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 1d2ddd543133, 763d4b211ec9 -> fee7b758c130, empty message
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade fee7b758c130 -> 867bf4f117f9, Adding extra field to Database model
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 867bf4f117f9 -> bb51420eaf83, add schema to table model
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade bb51420eaf83 -> b4456560d4f3, change_table_unique_constraint
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade b4456560d4f3 -> 4fa88fe24e94, owners_many_to_many
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Creating table dashboard_user...
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Table dashboard_user created.
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Creating table slice_user...
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Table slice_user created.
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 4fa88fe24e94 -> c3a8f8611885, Materializing permission
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade c3a8f8611885 -> f0fbf6129e13, Adding verbose_name to tablecolumn
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade f0fbf6129e13 -> 956a063c52b3, adjusting key length
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 956a063c52b3 -> 1226819ee0e3, Fix wrong constraint on table columns
2025-04-28 18:34:31 superset_init         | WARNI [root] Could not find or drop constraint on `columns`
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 1226819ee0e3 -> d8bc074f7aad, Add new field 'is_restricted' to SqlMetric and DruidMetric
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade d8bc074f7aad -> 27ae655e4247, Make creator owners
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 27ae655e4247 -> 960c69cb1f5b, add dttm_format related fields in table_columns
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 960c69cb1f5b -> f162a1dea4c4, d3format_by_metric
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade f162a1dea4c4 -> ad82a75afd82, Update models to support storing the queries.
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Creating table query...
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Table query created.
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade ad82a75afd82 -> 3c3ffe173e4f, add_sql_string_to_table
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 3c3ffe173e4f -> 41f6a59a61f2, database options for sql lab
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 41f6a59a61f2 -> 4500485bde7d, allow_run_sync_async
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 4500485bde7d -> 65903709c321, allow_dml
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 41f6a59a61f2 -> 33d996bcc382, update slice model
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 33d996bcc382, 65903709c321 -> b347b202819b, empty message
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade b347b202819b -> 5e4a03ef0bf0, Add access_request table to manage requests to access datastores.
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Creating table access_request...
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Table access_request created.
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 5e4a03ef0bf0 -> eca4694defa7, sqllab_setting_defaults
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade eca4694defa7 -> ab3d66c4246e, add_cache_timeout_to_druid_cluster
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade eca4694defa7 -> 3b626e2a6783, Sync DB with the models.py.
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 3b626e2a6783, ab3d66c4246e -> ef8843b41dac, empty message
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade ef8843b41dac -> b46fa1b0b39e, Add json_metadata to the tables table.
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade b46fa1b0b39e -> 7e3ddad2a00b, results_key to query
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 7e3ddad2a00b -> ad4d656d92bc, Add avg() to default metrics
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade ad4d656d92bc -> c611f2b591b8, dim_spec
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade c611f2b591b8 -> e46f2d27a08e, materialize perms
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade e46f2d27a08e -> f1f2d4af5b90, Enable Filter Select
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade e46f2d27a08e -> 525c854f0005, log_this_plus
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 525c854f0005, f1f2d4af5b90 -> 6414e83d82b7, empty message
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 6414e83d82b7 -> 1296d28ec131, Adds params to the datasource (druid) table
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 1296d28ec131 -> f18570e03440, Add index on the result key to the query table.
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade f18570e03440 -> bcf3126872fc, Add keyvalue table
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Creating table keyvalue...
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Table keyvalue created.
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade f18570e03440 -> db0c65b146bd, update_slice_model_json
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade db0c65b146bd -> a99f2f7c195a, rewriting url from shortener with new format
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade a99f2f7c195a, bcf3126872fc -> d6db5a5cdb5d, empty message
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade d6db5a5cdb5d -> b318dfe5fb6c, adding verbose_name to druid column
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade d6db5a5cdb5d -> 732f1c06bcbf, add fetch values predicate
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 732f1c06bcbf, b318dfe5fb6c -> ea033256294a, empty message
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade b318dfe5fb6c -> db527d8c4c78, Add verbose name to DruidCluster and Database
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade db527d8c4c78, ea033256294a -> 979c03af3341, empty message
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 979c03af3341 -> a6c18f869a4e, query.start_running_time
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade a6c18f869a4e -> 2fcdcb35e487, saved_queries
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 2fcdcb35e487 -> a65458420354, add_result_backend_time_logging
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade a65458420354 -> ca69c70ec99b, tracking_url
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade ca69c70ec99b -> a9c47e2c1547, add impersonate_user to dbs
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade ca69c70ec99b -> ddd6ebdd853b, annotations
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Creating table annotation_layer...
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Table annotation_layer created.
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Creating table annotation...
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Table annotation created.
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade a9c47e2c1547, ddd6ebdd853b -> d39b1e37131d, empty message
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade ca69c70ec99b -> 19a814813610, Adding metric warning_text
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 19a814813610, a9c47e2c1547 -> 472d2f73dfd4, empty message
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 472d2f73dfd4, d39b1e37131d -> f959a6652acd, empty message
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade f959a6652acd -> 4736ec66ce19, empty message
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 4736ec66ce19 -> 67a6ac9b727b, update_spatial_params
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 67a6ac9b727b -> 21e88bc06c02, migrate_old_annotation_layers
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 21e88bc06c02 -> e866bd2d4976, smaller_grid
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade e866bd2d4976 -> e68c4473c581, allow_multi_schema_metadata_fetch
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade e68c4473c581 -> f231d82b9b26, empty message
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade f231d82b9b26 -> bf706ae5eb46, cal_heatmap_metric_to_metrics
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade f231d82b9b26 -> 30bb17c0dc76, empty message
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 30bb17c0dc76, bf706ae5eb46 -> c9495751e314, empty message
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade f231d82b9b26 -> 130915240929, is_sqllab_view
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 130915240929, c9495751e314 -> 5ccf602336a0, empty message
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 5ccf602336a0 -> e502db2af7be, add template_params to tables
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade e502db2af7be -> c5756bec8b47, Time grain SQLA
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade c5756bec8b47 -> afb7730f6a9c, remove empty filters
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade afb7730f6a9c -> 80a67c5192fa, single pie chart metric
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 80a67c5192fa -> bddc498dd179, adhoc filters
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade bddc498dd179 -> 4451805bbaa1, remove double percents
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade bddc498dd179 -> 3dda56f1c4c6, Migrate num_period_compare and period_ratio_type
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 3dda56f1c4c6 -> 1d9e835a84f9, empty message
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 4451805bbaa1, 1d9e835a84f9 -> e3970889f38e, empty message
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 4451805bbaa1, 1d9e835a84f9 -> 705732c70154, empty message
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 4451805bbaa1, 1d9e835a84f9 -> fc480c87706c, empty message
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade fc480c87706c -> bebcf3fed1fe, Migrate dashboard position_json data from V1 to V2
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade bebcf3fed1fe, 705732c70154 -> ec1f88a35cc6, empty message
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 705732c70154, e3970889f38e -> 46ba6aaaac97, empty message
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 46ba6aaaac97, ec1f88a35cc6 -> c18bd4186f15, empty message
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade c18bd4186f15 -> 7fcdcde0761c, Reduce position_json size by remove extra space and component id prefix
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 7fcdcde0761c -> 0c5070e96b57, add user attributes table
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 0c5070e96b57 -> 1a1d627ebd8e, position_json
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 1a1d627ebd8e -> 55e910a74826, add_metadata_column_to_annotation_model.py
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 55e910a74826 -> 4ce8df208545, empty message
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 4ce8df208545 -> 46f444d8b9b7, remove_coordinator_from_druid_cluster_model.py
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 46f444d8b9b7 -> a61b40f9f57f, remove allow_run_sync
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade a61b40f9f57f -> 6c7537a6004a, models for email reports
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Creating table dashboard_email_schedules...
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Table dashboard_email_schedules created.
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Creating table slice_email_schedules...
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Table slice_email_schedules created.
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 6c7537a6004a -> 3e1b21cd94a4, change_owner_to_m2m_relation_on_datasources.py
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Creating table sqlatable_user...
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Table sqlatable_user created.
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Creating table druiddatasource_user...
2025-04-28 18:34:31 superset_init         | INFO  [alembic] Table druiddatasource_user created.
2025-04-28 18:34:31 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 6c7537a6004a -> cefabc8f7d38, Increase size of name column in ab_view_menu
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 55e910a74826 -> 0b1f1ab473c0, Add extra column to Query
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 0b1f1ab473c0, cefabc8f7d38, 3e1b21cd94a4 -> de021a1ca60d, empty message
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade de021a1ca60d -> fb13d49b72f9, better_filters
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade fb13d49b72f9 -> a33a03f16c4a, Add extra column to SavedQuery
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 4451805bbaa1, 1d9e835a84f9 -> c829ff0b37d0, empty message
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade c829ff0b37d0 -> 7467e77870e4, remove_aggs
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 7467e77870e4, de021a1ca60d -> fbd55e0f83eb, empty message
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade fbd55e0f83eb, fb13d49b72f9 -> 8b70aa3d0f87, empty message
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 8b70aa3d0f87, a33a03f16c4a -> 18dc26817ad2, empty message
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 18dc26817ad2 -> c617da68de7d, form nullable
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade c617da68de7d -> c82ee8a39623, Add implicit tags
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 18dc26817ad2 -> e553e78e90c5, add_druid_auth_py.py
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade e553e78e90c5, c82ee8a39623 -> 45e7da7cfeba, empty message
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 45e7da7cfeba -> 80aa3f04bc82, Add Parent ids in dashboard layout metadata
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 80aa3f04bc82 -> d94d33dbe938, form strip
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade d94d33dbe938 -> 937d04c16b64, update datasources
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 937d04c16b64 -> 7f2635b51f5d, update base columns
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 7f2635b51f5d -> e9df189e5c7e, update base metrics
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade e9df189e5c7e -> afc69274c25a, update the sql, select_sql, and executed_sql columns in the
2025-04-28 18:34:32 superset_init         |    query table in mysql dbs to be long text columns
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade afc69274c25a -> d7c1a0d6f2da, Remove limit used from query model
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade d7c1a0d6f2da -> ab8c66efdd01, resample
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade ab8c66efdd01 -> b4a38aa87893, deprecate database expression
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade b4a38aa87893 -> d6ffdf31bdd4, Add published column to dashboards
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade d6ffdf31bdd4 -> 190188938582, Remove duplicated entries in dashboard_slices table and add unique constraint
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 190188938582 -> def97f26fdfb, Add index to tagged_object
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade def97f26fdfb -> 11c737c17cc6, deprecate_restricted_metrics
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 11c737c17cc6 -> 258b5280a45e, form strip leading and trailing whitespace
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 258b5280a45e -> 1495eb914ad3, time range
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 1495eb914ad3 -> b6fa807eac07, make_names_non_nullable
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade b6fa807eac07 -> cca2f5d568c8, add encrypted_extra to dbs
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade cca2f5d568c8 -> c2acd2cf3df2, alter type of dbs encrypted_extra
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade c2acd2cf3df2 -> 78ee127d0d1d, reconvert legacy filters into adhoc
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 78ee127d0d1d -> db4b49eb0782, Add tables for SQL Lab state
2025-04-28 18:34:32 superset_init         | INFO  [alembic] Creating table tab_state...
2025-04-28 18:34:32 superset_init         | INFO  [alembic] Table tab_state created.
2025-04-28 18:34:32 superset_init         | INFO  [alembic] Creating table table_schema...
2025-04-28 18:34:32 superset_init         | INFO  [alembic] Table table_schema created.
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade db4b49eb0782 -> 5afa9079866a, serialize_schema_permissions.py
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 5afa9079866a -> 89115a40e8ea, Change table schema description to long text
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 89115a40e8ea -> 817e1c9b09d0, add_not_null_to_dbs_sqlalchemy_url
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 817e1c9b09d0 -> e96dbf2cfef0, datasource_cluster_fk
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade e96dbf2cfef0 -> 3325d4caccc8, empty message
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 3325d4caccc8 -> 0a6f12f60c73, add_role_level_security
2025-04-28 18:34:32 superset_init         | INFO  [alembic] Creating table row_level_security_filters...
2025-04-28 18:34:32 superset_init         | INFO  [alembic] Table row_level_security_filters created.
2025-04-28 18:34:32 superset_init         | INFO  [alembic] Creating table rls_filter_roles...
2025-04-28 18:34:32 superset_init         | INFO  [alembic] Table rls_filter_roles created.
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 0a6f12f60c73 -> 72428d1ea401, Add tmp_schema_name to the query object.
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 72428d1ea401 -> b5998378c225, add certificate to dbs
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade b5998378c225 -> f9a30386bd74, cleanup_time_granularity
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade f9a30386bd74 -> 620241d1153f, update time_grain_sqla
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 620241d1153f -> 743a117f0d98, Add slack to the schedule
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 743a117f0d98 -> e557699a813e, add_tables_relation_to_row_level_security
2025-04-28 18:34:32 superset_init         | INFO  [alembic] Creating table rls_filter_tables...
2025-04-28 18:34:32 superset_init         | INFO  [alembic] Table rls_filter_tables created.
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade e557699a813e -> ea396d202291, Add ctas_method to the Query object
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade ea396d202291 -> a72cb0ebeb22, deprecate dbs.perm column
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade a72cb0ebeb22 -> 2f1d15e8a6af, add_alerts
2025-04-28 18:34:32 superset_init         | INFO  [alembic] Creating table alerts...
2025-04-28 18:34:32 superset_init         | INFO  [alembic] Table alerts created.
2025-04-28 18:34:32 superset_init         | INFO  [alembic] Creating table alert_logs...
2025-04-28 18:34:32 superset_init         | INFO  [alembic] Table alert_logs created.
2025-04-28 18:34:32 superset_init         | INFO  [alembic] Creating table alert_owner...
2025-04-28 18:34:32 superset_init         | INFO  [alembic] Table alert_owner created.
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 2f1d15e8a6af -> f2672aa8350a, add_slack_to_alerts
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade f2672aa8350a -> f120347acb39, Add extra column to tables and metrics
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade f2672aa8350a -> 978245563a02, Migrate iframe in dashboard to markdown component
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 978245563a02, f120347acb39 -> f80a3b88324b, empty message
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade f80a3b88324b -> 2e5a0ee25ed4, refractor_alerting
2025-04-28 18:34:32 superset_init         | INFO  [alembic] Creating table alert_validators...
2025-04-28 18:34:32 superset_init         | INFO  [alembic] Table alert_validators created.
2025-04-28 18:34:32 superset_init         | INFO  [alembic] Creating table sql_observers...
2025-04-28 18:34:32 superset_init         | INFO  [alembic] Table sql_observers created.
2025-04-28 18:34:32 superset_init         | INFO  [alembic] Creating table sql_observations...
2025-04-28 18:34:32 superset_init         | INFO  [alembic] Table sql_observations created.
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade f80a3b88324b -> 175ea3592453, Add cache to datasource lookup table.
2025-04-28 18:34:32 superset_init         | INFO  [alembic] Creating table cache_keys...
2025-04-28 18:34:32 superset_init         | INFO  [alembic] Table cache_keys created.
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 175ea3592453, 2e5a0ee25ed4 -> ae19b4ee3692, empty message
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade ae19b4ee3692 -> e5ef6828ac4e, add rls filter type and grouping key
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade e5ef6828ac4e -> 3fbbc6e8d654, fix data access permissions for virtual datasets
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 3fbbc6e8d654 -> 18532d70ab98, Delete table_name unique constraint in mysql
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 18532d70ab98 -> b56500de1855, add_uuid_column_to_import_mixin
2025-04-28 18:34:32 superset_init         | 
2025-04-28 18:34:32 superset_init         | Cleaning up slice uuid from dashboard positionCleaning up slice uuid from dashboard position json.. Done.      
2025-04-28 18:34:32 superset_init         | 
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade b56500de1855 -> af30ca79208f, Collapse alerting models into a single one
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade af30ca79208f -> 585b0b1a7b18, add exec info to saved queries
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 585b0b1a7b18 -> 96e99fb176a0, add_import_mixing_to_saved_query
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 96e99fb176a0 -> 49b5a32daba5, add report schedules
2025-04-28 18:34:32 superset_init         | INFO  [alembic] Creating table report_schedule...
2025-04-28 18:34:32 superset_init         | INFO  [alembic] Table report_schedule created.
2025-04-28 18:34:32 superset_init         | INFO  [alembic] Creating table report_execution_log...
2025-04-28 18:34:32 superset_init         | INFO  [alembic] Table report_execution_log created.
2025-04-28 18:34:32 superset_init         | INFO  [alembic] Creating table report_recipient...
2025-04-28 18:34:32 superset_init         | INFO  [alembic] Table report_recipient created.
2025-04-28 18:34:32 superset_init         | INFO  [alembic] Creating table report_schedule_user...
2025-04-28 18:34:32 superset_init         | INFO  [alembic] Table report_schedule_user created.
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 49b5a32daba5 -> a8173232b786, Add path to logs
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade a8173232b786 -> e38177dbf641, security converge saved queries
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade e38177dbf641 -> 8ee129739cf9, security converge css templates
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 8ee129739cf9 -> 811494c0cc23, Remove path, path_no_int, and ref from logs
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 811494c0cc23 -> 5daced1f0e76, reports add working_timeout column
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 5daced1f0e76 -> 40f16acf1ba7, security converge reports
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 40f16acf1ba7 -> ccb74baaa89b, security converge charts
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade ccb74baaa89b -> c25cb2c78727, security converge annotations
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade c25cb2c78727 -> 45731db65d9c, security converge datasets
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 45731db65d9c -> 4b84f97828aa, security converge logs
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 4b84f97828aa -> 1f6dca87d1a2, security converge dashboards
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 1f6dca87d1a2 -> 42b4c9e01447, security converge databases
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 42b4c9e01447 -> e37912a26567, security converge queries
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade e37912a26567 -> ab104a954a8f, reports alter crontab size
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade ab104a954a8f -> 73fd22e742ab, add_dynamic_plugins.py
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 73fd22e742ab -> c878781977c6, alert reports shared uniqueness
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade c878781977c6 -> 260bf0649a77, migrate [x dateunit] to [x dateunit ago/later]
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 260bf0649a77 -> e11ccdd12658, add roles relationship to dashboard
2025-04-28 18:34:32 superset_init         | INFO  [alembic] Creating table dashboard_roles...
2025-04-28 18:34:32 superset_init         | INFO  [alembic] Table dashboard_roles created.
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade e11ccdd12658 -> 41ce8799acc3, rename pie label type
2025-04-28 18:34:32 superset_init         | Updated 0 pie chart labels.
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 41ce8799acc3 -> 070c043f2fdb, add granularity to charts where missing
2025-04-28 18:34:32 superset_init         | 0 slices altered
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 070c043f2fdb -> c501b7c653a3, add missing uuid column
2025-04-28 18:34:32 superset_init         | 
2025-04-28 18:34:32 superset_init         | Cleaning up slice uuid from dashboard positionCleaning up slice uuid from dashboard position json.. Done.      
2025-04-28 18:34:32 superset_init         | 
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade c501b7c653a3 -> 1412ec1e5a7b, legacy force directed to echart
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 1412ec1e5a7b -> 67da9ef1ef9c, add hide_left_bar to tabstate
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 67da9ef1ef9c -> 989bbe479899, rename_filter_configuration_in_dashboard_metadata.py
2025-04-28 18:34:32 superset_init         | Updated 0 native filter configurations.
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 989bbe479899 -> 301362411006, add_execution_id_to_report_execution_log_model.py
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 301362411006 -> 134cea61c5e7, remove dataset health check message
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 134cea61c5e7 -> 085f06488938, Country map use lowercase country name
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 085f06488938 -> fc3a3a8ff221, migrate filter sets to new format
2025-04-28 18:34:32 superset_init         | Updated 0 filter sets with 0 filters.
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade fc3a3a8ff221 -> 19e978e1b9c3, add_report_format_to_report_schedule_model.py
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 19e978e1b9c3 -> d416d0d715cc, add_limiting_factor_column_to_query_model.py
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade d416d0d715cc -> f1410ed7ec95, migrate native filters to new schema
2025-04-28 18:34:32 superset_init         | Upgraded 0 filters and 0 filter sets.
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade f1410ed7ec95 -> 453530256cea, add_save_form_column_to_db_model
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 453530256cea -> 3317e9248280, add_creation_method_to_reports_model
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 3317e9248280 -> 030c840e3a1c, Add query context to slices
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 030c840e3a1c -> ae1ed299413b, add_timezone_to_report_schedule
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade ae1ed299413b -> 31b2a1039d4a, drop tables constraint
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 31b2a1039d4a -> e323605f370a, fix schemas_allowed_for_csv_upload
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade e323605f370a -> 143b6f2815da, migrate pivot table v2 heatmaps to new format
2025-04-28 18:34:32 superset_init         | Upgraded 0 slices.
2025-04-28 18:34:32 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 143b6f2815da -> f6196627326f, update chart permissions
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade f6196627326f -> 6d20ba9ecb33, add_last_saved_at_to_slice_model
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 6d20ba9ecb33 -> 07071313dd52, change_fetch_values_predicate_to_text
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 07071313dd52 -> 021b81fe4fbb, Add type to native filter configuration
2025-04-28 18:34:33 superset_init         | INFO  [alembic] [AddTypeToNativeFilter] Starting upgrade
2025-04-28 18:34:33 superset_init         | INFO  [alembic] [AddTypeToNativeFilter] Done!
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 021b81fe4fbb -> 181091c0ef16, add_extra_column_to_columns_model
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 181091c0ef16 -> 3ebe0993c770, add filter set model
2025-04-28 18:34:33 superset_init         | INFO  [alembic] Creating table filter_sets...
2025-04-28 18:34:33 superset_init         | INFO  [alembic] Table filter_sets created.
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 3ebe0993c770 -> 60dc453f4e2e, migrate timeseries_limit_metric to legacy_order_by in pivot_table_v2
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 60dc453f4e2e -> 32646df09c64, update time grain SQLA
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 32646df09c64 -> f9847149153d, add_certifications_columns_to_slice
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade f9847149153d -> aea15018d53b, add_certifications_columns_to_dashboard
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade aea15018d53b -> b92d69a6643c, rename_csv_to_file
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade b92d69a6643c -> 0ca9e5f1dacd, rename to schemas_allowed_for_file_upload in dbs.extra
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 0ca9e5f1dacd -> abe27eaf93db, add_extra_config_column_to_alerts
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade abe27eaf93db -> 3ba29ecbaac5, Change datatype of type in BaseColumn
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 3ba29ecbaac5 -> fe23025b9441, rename_big_viz_total_form_data_fields
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade fe23025b9441 -> 31bb738bd1d2, move_pivot_table_v2_legacy_order_by_to_timeseries_limit_metric
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 31bb738bd1d2 -> bb38f40aa3ff, Add force_screenshot to alerts/reports
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade bb38f40aa3ff -> c53bae8f08dd, add_saved_query_foreign_key_to_tab_state
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade c53bae8f08dd -> 5fd49410a97a, Add columns for external management
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 5fd49410a97a -> 5afbb1a5849b, add_embedded_dashboard_table
2025-04-28 18:34:33 superset_init         | INFO  [alembic] Creating table embedded_dashboards...
2025-04-28 18:34:33 superset_init         | INFO  [alembic] Table embedded_dashboards created.
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 5afbb1a5849b -> b8d3a24d9131, New dataset models
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade b8d3a24d9131 -> b5a422d8e252, fix query and saved_query null schema
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade b5a422d8e252 -> ab9a9d86e695, deprecate time_range_endpoints
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade ab9a9d86e695 -> 7293b0ca7944, change_adhoc_filter_b_from_none_to_empty_array
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 7293b0ca7944 -> 6766938c6065, add key-value store
2025-04-28 18:34:33 superset_init         | INFO  [alembic] Creating table key_value...
2025-04-28 18:34:33 superset_init         | INFO  [alembic] Table key_value created.
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 6766938c6065 -> 58df9d617f14, add_on_saved_query_delete_tab_state_null_constraint"
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 58df9d617f14 -> 2ed890b36b94, rm_time_range_endpoints_from_qc
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 2ed890b36b94 -> b0d0249074e4, deprecate time_range_endpoints v2
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 2ed890b36b94 -> 8b841273bec3, sql_lab_models_database_constraint_updates
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 8b841273bec3, b0d0249074e4 -> 9d8a8d575284, merge point
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 9d8a8d575284 -> cecc6bf46990, rm_time_range_endpoints_2
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade cecc6bf46990 -> ad07e4fdbaba, rm_time_range_endpoints_from_qc_3
2025-04-28 18:34:33 superset_init         | slices updated with no time_range_endpoints: 0
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade ad07e4fdbaba -> a9422eeaae74, new_dataset_models_take_2
2025-04-28 18:34:33 superset_init         | >> Assign new UUIDs to tables...
2025-04-28 18:34:33 superset_init         | >> Drop intermediate columns...
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade a9422eeaae74 -> cbe71abde154, fix report schedule and execution log
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade cbe71abde154 -> 6f139c533bea, adding advanced data type to column models
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 6f139c533bea -> e786798587de, Delete None permissions
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade e786798587de -> e09b4ae78457, Resize key_value blob
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade e09b4ae78457 -> f3afaf1f11f0, add_unique_name_desc_rls
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade f3afaf1f11f0 -> 7fb8bca906d2, permalink_rename_filterState
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 7fb8bca906d2 -> cdcf3d64daf4, Add user_id and dttm composite index to Log model
2025-04-28 18:34:33 superset_init         | INFO  [alembic] Creating index ix_logs_user_id_dttm on table logs
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade cdcf3d64daf4 -> c747c78868b6, Migrating legacy TreeMap
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade c747c78868b6 -> 06e1e70058c7, Migrating legacy Area
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 06e1e70058c7 -> a39867932713, query_context_to_mediumtext
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade a39867932713 -> 409c7b420ab0, add created_by_fk as owner
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 409c7b420ab0 -> ffa79af61a56, rename report_schedule.extra to extra_json
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade ffa79af61a56 -> 6d3c6f9d665d, fix_table_chart_conditional_formatting_colors
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 6d3c6f9d665d -> 291f024254b5, drop_column_allow_multi_schema_metadata_fetch
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 291f024254b5 -> deb4c9d4a4ef, parameters in saved queries
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade deb4c9d4a4ef -> 4ce1d9b25135, remove_filter_bar_orientation
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 4ce1d9b25135 -> f3c2d8ec8595, create_ssh_tunnel_credentials_tbl
2025-04-28 18:34:33 superset_init         | INFO  [alembic] Creating table ssh_tunnels...
2025-04-28 18:34:33 superset_init         | INFO  [alembic] Table ssh_tunnels created.
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade f3c2d8ec8595 -> 9c2a5681ddfd, convert key-value entries to json
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 9c2a5681ddfd -> c0a3ea245b61, remove_show_native_filters
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade c0a3ea245b61 -> d0ac08bb5b83, invert_horizontal_bar_chart_order
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade d0ac08bb5b83 -> b5ea9d343307, bar_chart_stack_options
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade b5ea9d343307 -> 07f9a902af1b, drop postgres enum constrains for tags
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 07f9a902af1b -> 7e67aecbf3f1, chart-ds-constraint
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 7e67aecbf3f1 -> 4ea966691069, cross-filter-global-scoping
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 4ea966691069 -> 9ba2ce3086e5, migrate-pivot-table-v1-to-v2
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 9ba2ce3086e5 -> 4c5da39be729, migrate_treemap_chart
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 4c5da39be729 -> ae58e1e58e5c, migrate_dual_line_to_mixed_chart
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade ae58e1e58e5c -> 83e1abbe777f, drop access_request
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 83e1abbe777f -> 90139bf715e4, add_currency_column_to_metrics
2025-04-28 18:34:33 superset_init         | INFO  [alembic] Adding column currency to table metrics...
2025-04-28 18:34:33 superset_init         | INFO  [alembic] Adding column currency to table sql_metrics...
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 90139bf715e4 -> 6fbe660cac39, add on delete cascade for tables references
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 6fbe660cac39 -> 8e5b0fb85b9a, Add custom size columns to report schedule
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 8e5b0fb85b9a -> 240d23c7f86f, update_tag_model_w_description
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 240d23c7f86f -> f92a3124dd66, drop rouge constraints and tables
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade f92a3124dd66 -> 6d05b0a70c89, add on delete cascade for owners references
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 6d05b0a70c89 -> 863adcf72773, delete obsolete Druid NoSQL slice parameters
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 863adcf72773 -> a23c6f8b1280, cleanup erroneous parent filter IDs
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade a23c6f8b1280 -> bf646a0c1501, json_metadata
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade bf646a0c1501 -> e0f6f91c2055, create_user_favorite_table
2025-04-28 18:34:33 superset_init         | INFO  [alembic] Creating table user_favorite_tag...
2025-04-28 18:34:33 superset_init         | INFO  [alembic] Table user_favorite_tag created.
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade e0f6f91c2055 -> ee179a490af9, deckgl-path-width-units
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade ee179a490af9 -> 0769ef90fddd, Fix schema perm for datasets
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 0769ef90fddd -> 2e826adca42c, Fix schema for log
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 2e826adca42c -> 8ace289026f3, add on delete cascade for dashboard_slices
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 8ace289026f3 -> 4448fa6deeb1, add on delete cascade for embedded_dashboards
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 4448fa6deeb1 -> 9f4a086c2676, add_normalize_columns_to_sqla_model
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 9f4a086c2676 -> ec54aca4c8a2, Increase ab_user.email field size
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade ec54aca4c8a2 -> 317970b4400c, Added always_filter_main_dttm to datasource
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 317970b4400c -> 4b85906e5b91, add on delete cascade for dashboard_roles
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 4b85906e5b91 -> b7851ee5522f, replay 317970b4400c
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade b7851ee5522f -> 06dd9ff00fe8, add_percent_calculation_type_funnel_chart
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 06dd9ff00fe8 -> 65a167d4c62e, add indexes to report models
2025-04-28 18:34:33 superset_init         | INFO  [alembic] Creating index ix_report_execution_log_report_schedule_id on table report_execution_log
2025-04-28 18:34:33 superset_init         | INFO  [alembic] Creating index ix_report_execution_log_start_dttm on table report_execution_log
2025-04-28 18:34:33 superset_init         | INFO  [alembic] Creating index ix_report_recipient_report_schedule_id on table report_recipient
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 65a167d4c62e -> 59a1450b3c10, drop_filter_sets_table
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 59a1450b3c10 -> a32e0c4d8646, migrate-sunburst-chart
2025-04-28 18:34:33 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 59a1450b3c10 -> 96164e3017c6
2025-04-28 18:34:34 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 96164e3017c6, a32e0c4d8646 -> 15a2c68a2e6b, merging two heads
2025-04-28 18:34:34 superset_init         | INFO  [alembic.runtime.migration] Running upgrade a32e0c4d8646 -> 214f580d09c9, migrate_filter_boxes_to_native_filters
2025-04-28 18:34:34 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 214f580d09c9 -> e863403c0c50, drop_url_table
2025-04-28 18:34:34 superset_init         | INFO  [alembic.runtime.migration] Running upgrade e863403c0c50, 15a2c68a2e6b -> 1cf8e4344e2b, merging
2025-04-28 18:34:34 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 1cf8e4344e2b -> 87d38ad83218, Migrate can_view_and_drill permission
2025-04-28 18:34:34 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 87d38ad83218 -> 17fcea065655, change_text_to_mediumtext
2025-04-28 18:34:34 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 17fcea065655 -> be1b217cd8cd, big_number_kpi_single_metric
2025-04-28 18:34:34 superset_init         | INFO  [alembic.runtime.migration] Running upgrade be1b217cd8cd -> 678eefb4ab44, Add access token table
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Creating table database_user_oauth2_tokens...
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Table database_user_oauth2_tokens created.
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Creating index idx_user_id_database_id on table database_user_oauth2_tokens
2025-04-28 18:34:34 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 678eefb4ab44 -> c22cb5c2e546, empty message
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Adding column avatar_url to table user_attribute...
2025-04-28 18:34:34 superset_init         | INFO  [alembic.runtime.migration] Running upgrade c22cb5c2e546 -> 5ad7321c2169, mig new csv upload perm
2025-04-28 18:34:34 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 5ad7321c2169 -> d60591c5515f, mig new excel upload perm
2025-04-28 18:34:34 superset_init         | INFO  [alembic.runtime.migration] Running upgrade d60591c5515f -> 5f57af97bc3f, Add catalog column
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Adding column catalog to table tables...
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Adding column catalog to table query...
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Adding column catalog to table saved_query...
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Adding column catalog to table tab_state...
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Adding column catalog to table table_schema...
2025-04-28 18:34:34 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 5f57af97bc3f -> 3dfd0e78650e, add_query_sql_editor_id_index
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Creating index ix_sql_editor_id on table query
2025-04-28 18:34:34 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 5f57af97bc3f -> 4a33124c18ad, mig new columnar upload perm
2025-04-28 18:34:34 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 4a33124c18ad -> 58d051681a3b, Add catalog_perm to tables
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Adding column catalog_perm to table tables...
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Adding column catalog_perm to table slices...
2025-04-28 18:34:34 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 58d051681a3b, 3dfd0e78650e -> 645bb206f96c, empty message
2025-04-28 18:34:34 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 645bb206f96c -> 4081be5b6b74, Enable catalog in Databricks
2025-04-28 18:34:34 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 4081be5b6b74 -> 87ffc36f9842, Enable catalog in BigQuery/Presto/Trino/Snowflake
2025-04-28 18:34:34 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 87ffc36f9842 -> 9621c6d56ffb, add subject column to report schedule
2025-04-28 18:34:34 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 9621c6d56ffb -> f84fde59123a, Update charts with old time comparison controls
2025-04-28 18:34:34 superset_init         | INFO  [alembic.runtime.migration] Running upgrade f84fde59123a -> f7b6750b67e8, change_mediumtext_to_longtext
2025-04-28 18:34:34 superset_init         | Revision ID: f7b6750b67e8
2025-04-28 18:34:34 superset_init         | Revises: f84fde59123a
2025-04-28 18:34:34 superset_init         | Create Date: 2024-05-09 19:19:46.630140
2025-04-28 18:34:34 superset_init         | INFO  [alembic.runtime.migration] Running upgrade f7b6750b67e8 -> 02f4f7811799, remove sl_dataset_columns tables
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Dropping foreign key sl_dataset_columns_column_id_fkey from table sl_dataset_columns...
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Dropping foreign key sl_dataset_columns_dataset_id_fkey from table sl_dataset_columns...
2025-04-28 18:34:34 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 02f4f7811799 -> 39549add7bfc, remove sl_table_columns_table
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Dropping foreign key sl_table_columns_column_id_fkey from table sl_table_columns...
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Dropping foreign key sl_table_columns_table_id_fkey from table sl_table_columns...
2025-04-28 18:34:34 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 39549add7bfc -> 38f4144e8558, remove sl_dataset_tables
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Dropping foreign key sl_dataset_tables_table_id_fkey from table sl_dataset_tables...
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Dropping foreign key sl_dataset_tables_dataset_id_fkey from table sl_dataset_tables...
2025-04-28 18:34:34 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 38f4144e8558 -> e53fd48cc078, remove sl_dataset_users
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Dropping foreign key sl_dataset_users_dataset_id_fkey from table sl_dataset_users...
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Dropping foreign key sl_dataset_users_user_id_fkey from table sl_dataset_users...
2025-04-28 18:34:34 superset_init         | INFO  [alembic.runtime.migration] Running upgrade e53fd48cc078 -> a6b32d2d07b1, remove sl_columns
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Dropping foreign key sl_columns_changed_by_fk_fkey from table sl_columns...
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Dropping foreign key sl_columns_created_by_fk_fkey from table sl_columns...
2025-04-28 18:34:34 superset_init         | INFO  [alembic.runtime.migration] Running upgrade a6b32d2d07b1 -> 007a1abffe7e, remove sl_tables
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Dropping foreign key sl_tables_changed_by_fk_fkey from table sl_tables...
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Dropping foreign key sl_tables_database_id_fkey from table sl_tables...
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Dropping foreign key sl_tables_created_by_fk_fkey from table sl_tables...
2025-04-28 18:34:34 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 007a1abffe7e -> 48cbb571fa3a, remove sl_datasets
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Dropping foreign key sl_datasets_created_by_fk_fkey from table sl_datasets...
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Dropping foreign key sl_datasets_changed_by_fk_fkey from table sl_datasets...
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Dropping foreign key sl_datasets_database_id_fkey from table sl_datasets...
2025-04-28 18:34:34 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 48cbb571fa3a -> 7b17aa722e30, UUIDMixin
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Adding column uuid to table css_templates...
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Adding column uuid to table favstar...
2025-04-28 18:34:34 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 48cbb571fa3a -> df3d7e2eb9a4, Remove _customer_location_uc
2025-04-28 18:34:34 superset_init         | INFO  [alembic.runtime.migration] Running upgrade df3d7e2eb9a4, 7b17aa722e30 -> eb1c288c71c4, empty message
2025-04-28 18:34:34 superset_init         | INFO  [alembic.runtime.migration] Running upgrade eb1c288c71c4 -> d482d51c15ca, remove_legacy_plugins_5_0
2025-04-28 18:34:34 superset_init         | INFO  [alembic.runtime.migration] Running upgrade d482d51c15ca -> 74ad1125881c, converge_upload_permissions
2025-04-28 18:34:34 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 74ad1125881c -> 94e7a3499973, Add folders column to datasets
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Adding column folders to table tables...
2025-04-28 18:34:34 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 94e7a3499973 -> 32bf93dfe2a4, Add on cascade to foreign keys in ab_permission_view_role and ab_user_role
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Dropping foreign key ab_permission_view_role_permission_view_id_fkey from table ab_permission_view_role...
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Dropping foreign key ab_permission_view_role_role_id_fkey from table ab_permission_view_role...
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Creating foreign key ab_permission_view_role_role_id_fkey on table ab_permission_view_role...
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Creating foreign key ab_permission_view_role_permission_view_id_fkey on table ab_permission_view_role...
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Dropping foreign key ab_user_role_role_id_fkey from table ab_user_role...
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Dropping foreign key ab_user_role_user_id_fkey from table ab_user_role...
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Creating foreign key ab_user_role_user_id_fkey on table ab_user_role...
2025-04-28 18:34:34 superset_init         | INFO  [alembic] Creating foreign key ab_user_role_role_id_fkey on table ab_user_role...
2025-04-28 18:34:34 superset_init         | INFO  [alembic.runtime.migration] Running upgrade 32bf93dfe2a4 -> 378cecfdba9f, merge_x_axis_sort_series_with_x_axis_sort
2025-04-28 18:34:34 superset_init         | INFO  [alembic.env] Migration scripts completed. Duration: 00:00:03
2025-04-28 18:34:35 superset_init         | ######################################################################
2025-04-28 18:34:35 superset_init         | Init Step 1/4 [Complete] -- Applying DB migrations
2025-04-28 18:34:35 superset_init         | ######################################################################
2025-04-28 18:34:35 superset_init         | ######################################################################
2025-04-28 18:34:35 superset_init         | Init Step 2/4 [Starting] -- Setting up admin user ( admin / admin )
2025-04-28 18:34:35 superset_init         | ######################################################################
2025-04-28 18:34:37 superset_init         | Loaded your LOCAL configuration at [/app/docker/pythonpath_dev/superset_config.py]
2025-04-28 18:34:37 superset_init         | 2025-04-28 13:04:37,820:INFO:superset.initialization:Setting database isolation level to READ COMMITTED
2025-04-28 18:34:37 superset_init         | 2025-04-28 13:04:37,944:INFO:superset.utils.screenshots:No PIL installation found
2025-04-28 18:34:38 superset_init         | 2025-04-28 13:04:38,306:INFO:superset.utils.pdf:No PIL installation found
2025-04-28 18:34:38 superset_init         | Recognized Database Authentications.
2025-04-28 18:34:38 superset_init         | Admin User admin created.
2025-04-28 18:34:39 superset_init         | ######################################################################
2025-04-28 18:34:39 superset_init         | Init Step 2/4 [Complete] -- Setting up admin user
2025-04-28 18:34:39 superset_init         | ######################################################################
2025-04-28 18:34:39 superset_init         | ######################################################################
2025-04-28 18:34:39 superset_init         | Init Step 3/4 [Starting] -- Setting up roles and perms
2025-04-28 18:34:39 superset_init         | ######################################################################
2025-04-28 18:34:40 superset_init         | Loaded your LOCAL configuration at [/app/docker/pythonpath_dev/superset_config.py]
2025-04-28 18:34:40 superset_init         | 2025-04-28 13:04:40,964:INFO:superset.initialization:Setting database isolation level to READ COMMITTED
2025-04-28 18:34:41 superset_init         | 2025-04-28 13:04:41,084:INFO:superset.utils.screenshots:No PIL installation found
2025-04-28 18:34:41 superset_init         | 2025-04-28 13:04:41,503:INFO:superset.utils.pdf:No PIL installation found
2025-04-28 18:34:44 superset_init         | 2025-04-28 13:04:44,836:INFO:superset.security.manager:Syncing role definition
2025-04-28 18:34:44 superset_init         | 2025-04-28 13:04:44,956:INFO:superset.security.manager:Syncing Admin perms
2025-04-28 18:34:44 superset_init         | 2025-04-28 13:04:44,961:INFO:superset.security.manager:Syncing Alpha perms
2025-04-28 18:34:45 superset_init         | 2025-04-28 13:04:45,140:INFO:superset.security.manager:Syncing Gamma perms
2025-04-28 18:34:45 superset_init         | 2025-04-28 13:04:45,303:INFO:superset.security.manager:Syncing sql_lab perms
2025-04-28 18:34:45 superset_init         | 2025-04-28 13:04:45,462:INFO:superset.security.manager:Fetching a set of all perms to lookup which ones are missing
2025-04-28 18:34:45 superset_init         | 2025-04-28 13:04:45,469:INFO:superset.security.manager:Creating missing datasource permissions.
2025-04-28 18:34:45 superset_init         | 2025-04-28 13:04:45,475:INFO:superset.security.manager:Creating missing database permissions.
2025-04-28 18:34:45 superset_init         | 2025-04-28 13:04:45,481:INFO:superset.security.manager:Cleaning faulty perms
2025-04-28 18:34:46 superset_init         | ######################################################################
2025-04-28 18:34:46 superset_init         | Init Step 3/4 [Complete] -- Setting up roles and perms
2025-04-28 18:34:46 superset_init         | ######################################################################
2025-04-28 18:34:46 superset_init         | ######################################################################
2025-04-28 18:35:04 superset_worker       | Reinstalling the app in editable mode
2025-04-28 18:34:46 superset_init         | Init Step 4/4 [Starting] -- Loading examples
2025-04-28 18:34:46 superset_init         | ######################################################################
2025-04-28 18:34:47 superset_init         | Loaded your LOCAL configuration at [/app/docker/pythonpath_dev/superset_config.py]
2025-04-28 18:34:48 superset_init         | 2025-04-28 13:04:48,006:INFO:superset.initialization:Setting database isolation level to READ COMMITTED
2025-04-28 18:34:48 superset_init         | 2025-04-28 13:04:48,130:INFO:superset.utils.screenshots:No PIL installation found
2025-04-28 18:35:06 superset_worker       | Resolved 147 packages in 2.05s
2025-04-28 18:35:08 superset_worker       |    Building apache-superset @ file:///app
2025-04-28 18:34:48 superset_init         | 2025-04-28 13:04:48,453:INFO:superset.utils.pdf:No PIL installation found
2025-04-28 18:34:48 superset_init         | 2025-04-28 13:04:48,890:INFO:superset.utils.database:Creating database reference for examples
2025-04-28 18:34:48 superset_init         | 2025-04-28 13:04:48,905:INFO:superset.cli.examples:Loading examples metadata and related data into examples
2025-04-28 18:34:48 superset_init         | 2025-04-28 13:04:48,918:INFO:superset.cli.examples:Loading [World Bank's Health Nutrition and Population Stats]
2025-04-28 18:34:17 superset_cache        | 1:C 28 Apr 2025 13:04:17.477 # WARNING Memory overcommit must be enabled! Without it, a background save or replication may fail under low memory condition. Being disabled, it can also cause failures without low memory condition, see https://github.com/jemalloc/jemalloc/issues/1328. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
2025-04-28 18:34:17 superset_cache        | 1:C 28 Apr 2025 13:04:17.477 * oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
2025-04-28 18:34:17 superset_cache        | 1:C 28 Apr 2025 13:04:17.477 * Redis version=7.4.2, bits=64, commit=00000000, modified=0, pid=1, just started
2025-04-28 18:34:17 superset_cache        | 1:C 28 Apr 2025 13:04:17.477 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
2025-04-28 18:34:17 superset_cache        | 1:M 28 Apr 2025 13:04:17.478 * monotonic clock: POSIX clock_gettime
2025-04-28 18:34:17 superset_cache        | 1:M 28 Apr 2025 13:04:17.479 * Running mode=standalone, port=6379.
2025-04-28 18:34:17 superset_cache        | 1:M 28 Apr 2025 13:04:17.480 * Server initialized
2025-04-28 18:34:17 superset_cache        | 1:M 28 Apr 2025 13:04:17.480 * Ready to accept connections tcp
2025-04-28 18:34:17 superset_db           | The files belonging to this database system will be owned by user "postgres".
2025-04-28 18:34:17 superset_db           | This user must also own the server process.
2025-04-28 18:34:17 superset_db           | 
2025-04-28 18:34:17 superset_db           | The database cluster will be initialized with locale "en_US.utf8".
2025-04-28 18:34:17 superset_db           | The default database encoding has accordingly been set to "UTF8".
2025-04-28 18:34:17 superset_db           | The default text search configuration will be set to "english".
2025-04-28 18:34:17 superset_db           | 
2025-04-28 18:34:17 superset_db           | Data page checksums are disabled.
2025-04-28 18:34:17 superset_db           | 
2025-04-28 18:34:17 superset_db           | fixing permissions on existing directory /var/lib/postgresql/data ... ok
2025-04-28 18:34:17 superset_db           | creating subdirectories ... ok
2025-04-28 18:34:17 superset_db           | selecting dynamic shared memory implementation ... posix
2025-04-28 18:34:17 superset_db           | selecting default max_connections ... 100
2025-04-28 18:34:17 superset_db           | selecting default shared_buffers ... 128MB
2025-04-28 18:34:17 superset_db           | selecting default time zone ... Etc/UTC
2025-04-28 18:34:17 superset_db           | creating configuration files ... ok
2025-04-28 18:34:17 superset_db           | running bootstrap script ... ok
2025-04-28 18:34:18 superset_db           | performing post-bootstrap initialization ... ok
2025-04-28 18:34:18 superset_db           | initdb: warning: enabling "trust" authentication for local connections
2025-04-28 18:34:18 superset_db           | initdb: hint: You can change this by editing pg_hba.conf or using the option -A, or --auth-local and --auth-host, the next time you run initdb.
2025-04-28 18:34:18 superset_db           | syncing data to disk ... ok
2025-04-28 18:34:18 superset_db           | 
2025-04-28 18:34:18 superset_db           | 
2025-04-28 18:34:18 superset_db           | Success. You can now start the database server using:
2025-04-28 18:34:18 superset_db           | 
2025-04-28 18:34:18 superset_db           |     pg_ctl -D /var/lib/postgresql/data -l logfile start
2025-04-28 18:34:18 superset_db           | 
2025-04-28 18:34:18 superset_db           | waiting for server to start....2025-04-28 13:04:18.677 UTC [49] LOG:  starting PostgreSQL 16.8 (Debian 16.8-1.pgdg120+1) on x86_64-pc-linux-gnu, compiled by gcc (Debian 12.2.0-14) 12.2.0, 64-bit
2025-04-28 18:34:18 superset_db           | 2025-04-28 13:04:18.682 UTC [49] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
2025-04-28 18:34:18 superset_db           | 2025-04-28 13:04:18.708 UTC [52] LOG:  database system was shut down at 2025-04-28 13:04:18 UTC
2025-04-28 18:34:18 superset_db           | 2025-04-28 13:04:18.719 UTC [49] LOG:  database system is ready to accept connections
2025-04-28 18:34:18 superset_db           |  done
2025-04-28 18:34:18 superset_db           | server started
2025-04-28 18:34:18 superset_db           | CREATE DATABASE
2025-04-28 18:34:18 superset_db           | 
2025-04-28 18:34:18 superset_db           | 
2025-04-28 18:34:18 superset_db           | /usr/local/bin/docker-entrypoint.sh: running /docker-entrypoint-initdb.d/cypress-init.sh
2025-04-28 18:34:18 superset_db           | CREATE DATABASE
2025-04-28 18:34:18 superset_db           | 
2025-04-28 18:34:18 superset_db           | /usr/local/bin/docker-entrypoint.sh: running /docker-entrypoint-initdb.d/examples-init.sh
2025-04-28 18:34:19 superset_db           | CREATE ROLE
2025-04-28 18:34:19 superset_db           | CREATE DATABASE
2025-04-28 18:34:19 superset_db           | GRANT
2025-04-28 18:34:19 superset_db           | GRANT
2025-04-28 18:34:19 superset_db           | 
2025-04-28 18:34:19 superset_db           | waiting for server to shut down....2025-04-28 13:04:19.103 UTC [49] LOG:  received fast shutdown request
2025-04-28 18:34:19 superset_db           | 2025-04-28 13:04:19.106 UTC [49] LOG:  aborting any active transactions
2025-04-28 18:34:19 superset_db           | 2025-04-28 13:04:19.107 UTC [49] LOG:  background worker "logical replication launcher" (PID 55) exited with exit code 1
2025-04-28 18:34:19 superset_db           | 2025-04-28 13:04:19.108 UTC [50] LOG:  shutting down
2025-04-28 18:34:19 superset_db           | 2025-04-28 13:04:19.110 UTC [50] LOG:  checkpoint starting: shutdown immediate
2025-04-28 18:34:19 superset_db           | 2025-04-28 13:04:19.340 UTC [50] LOG:  checkpoint complete: wrote 2763 buffers (16.9%); 0 WAL file(s) added, 0 removed, 1 recycled; write=0.047 s, sync=0.178 s, total=0.232 s; sync files=900, longest=0.005 s, average=0.001 s; distance=12769 kB, estimate=12769 kB; lsn=0/21627F0, redo lsn=0/21627F0
2025-04-28 18:34:19 superset_db           | 2025-04-28 13:04:19.346 UTC [49] LOG:  database system is shut down
2025-04-28 18:34:19 superset_db           |  done
2025-04-28 18:34:19 superset_db           | server stopped
2025-04-28 18:34:19 superset_db           | 
2025-04-28 18:34:19 superset_db           | PostgreSQL init process complete; ready for start up.
2025-04-28 18:34:19 superset_db           | 
2025-04-28 18:34:19 superset_db           | 2025-04-28 13:04:19.425 UTC [1] LOG:  starting PostgreSQL 16.8 (Debian 16.8-1.pgdg120+1) on x86_64-pc-linux-gnu, compiled by gcc (Debian 12.2.0-14) 12.2.0, 64-bit
2025-04-28 18:34:19 superset_db           | 2025-04-28 13:04:19.425 UTC [1] LOG:  listening on IPv4 address "0.0.0.0", port 5432
2025-04-28 18:34:19 superset_db           | 2025-04-28 13:04:19.425 UTC [1] LOG:  listening on IPv6 address "::", port 5432
2025-04-28 18:34:19 superset_db           | 2025-04-28 13:04:19.429 UTC [1] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
2025-04-28 18:34:19 superset_db           | 2025-04-28 13:04:19.433 UTC [73] LOG:  database system was shut down at 2025-04-28 13:04:19 UTC
2025-04-28 18:34:19 superset_db           | 2025-04-28 13:04:19.438 UTC [1] LOG:  database system is ready to accept connections
2025-04-28 18:35:09 superset_worker       |       Built apache-superset @ file:///app
2025-04-28 18:35:09 superset_worker       | Prepared 1 package in 3.81s
2025-04-28 18:35:09 superset_worker_beat  | Prepared 1 package in 1.91s
2025-04-28 18:35:10 superset_worker       | Uninstalled 1 package in 190ms
2025-04-28 18:35:10 superset_worker       | warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
2025-04-28 18:35:10 superset_worker       |          If the cache and target directories are on different filesystems, hardlinking may not be supported.
2025-04-28 18:35:10 superset_worker       |          If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
2025-04-28 18:35:10 superset_worker       | Installed 1 package in 1ms
2025-04-28 18:35:10 superset_worker       |  ~ apache-superset==0.0.0.dev0 (from file:///app)
2025-04-28 18:35:10 superset_worker_beat  | Uninstalled 1 package in 190ms
2025-04-28 18:35:10 superset_worker_beat  | warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
2025-04-28 18:35:10 superset_worker_beat  |          If the cache and target directories are on different filesystems, hardlinking may not be supported.
2025-04-28 18:35:10 superset_worker_beat  |          If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
2025-04-28 18:35:10 superset_worker_beat  | Installed 1 package in 2ms
2025-04-28 18:35:10 superset_worker_beat  |  ~ apache-superset==0.0.0.dev0 (from file:///app)
2025-04-28 18:35:10 superset_worker       | Installing postgres requirements
2025-04-28 18:35:10 superset_worker_beat  | Installing postgres requirements
2025-04-28 18:35:11 superset_worker_beat  | Resolved 148 packages in 1.69s
2025-04-28 18:35:11 superset_worker_beat  |    Building apache-superset @ file:///app
2025-04-28 18:35:11 superset_worker       | Resolved 148 packages in 1.70s
2025-04-28 18:35:13 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:13 superset_app          |                                     WARNING
2025-04-28 18:35:13 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:13 superset_app          | A Default SECRET_KEY was detected, please use superset_config.py to override it.
2025-04-28 18:35:13 superset_app          | Use a strong complex alphanumeric string and use a tool to help you generate 
2025-04-28 18:35:13 superset_app          | a sufficiently random sequence, ex: openssl rand -base64 42 
2025-04-28 18:35:13 superset_app          | For more info, see: https://superset.apache.org/docs/configuration/configuring-superset#specifying-a-secret_key
2025-04-28 18:35:13 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:13 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:13 superset_app          | Refusing to start due to insecure SECRET_KEY
2025-04-28 18:35:13 superset_app          | [2025-04-28 13:05:13 +0000] [9] [INFO] Worker exiting (pid: 9)
2025-04-28 18:35:13 superset_app          | [2025-04-28 13:05:13 +0000] [8] [ERROR] Worker (pid:9) exited with code 1
2025-04-28 18:35:13 superset_app          | [2025-04-28 13:05:13 +0000] [8] [ERROR] Worker (pid:9) exited with code 1.
2025-04-28 18:35:13 superset_app          | [2025-04-28 13:05:13 +0000] [34] [INFO] Booting worker with pid: 34
2025-04-28 18:35:13 superset_worker_beat  |       Built apache-superset @ file:///app
2025-04-28 18:35:13 superset_worker_beat  | Prepared 1 package in 1.86s
2025-04-28 18:35:13 superset_worker       | Prepared 1 package in 1.86s
2025-04-28 18:35:13 superset_worker_beat  | Uninstalled 1 package in 0.78ms
2025-04-28 18:35:13 superset_worker_beat  | warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
2025-04-28 18:35:13 superset_worker_beat  |          If the cache and target directories are on different filesystems, hardlinking may not be supported.
2025-04-28 18:35:13 superset_worker_beat  |          If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
2025-04-28 18:35:13 superset_worker       | Uninstalled 1 package in 0.71ms
2025-04-28 18:35:13 superset_worker       | warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
2025-04-28 18:35:13 superset_worker       |          If the cache and target directories are on different filesystems, hardlinking may not be supported.
2025-04-28 18:35:13 superset_worker       |          If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
2025-04-28 18:35:13 superset_worker       | Installed 1 package in 1ms
2025-04-28 18:35:13 superset_worker       |  ~ apache-superset==0.0.0.dev0 (from file:///app)
2025-04-28 18:35:13 superset_worker_beat  | Installed 1 package in 3ms
2025-04-28 18:35:13 superset_worker_beat  |  ~ apache-superset==0.0.0.dev0 (from file:///app)
2025-04-28 18:35:13 superset_worker       | Skipping local overrides
2025-04-28 18:35:13 superset_worker       | Starting Celery worker...
2025-04-28 18:35:13 superset_worker_beat  | Skipping local overrides
2025-04-28 18:35:13 superset_worker_beat  | Starting Celery beat...
2025-04-28 18:35:14 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:14 superset_app          |                                     WARNING
2025-04-28 18:35:14 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:14 superset_app          | A Default SECRET_KEY was detected, please use superset_config.py to override it.
2025-04-28 18:35:14 superset_app          | Use a strong complex alphanumeric string and use a tool to help you generate 
2025-04-28 18:35:14 superset_app          | a sufficiently random sequence, ex: openssl rand -base64 42 
2025-04-28 18:35:14 superset_app          | For more info, see: https://superset.apache.org/docs/configuration/configuring-superset#specifying-a-secret_key
2025-04-28 18:35:14 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:14 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:14 superset_app          | Refusing to start due to insecure SECRET_KEY
2025-04-28 18:35:14 superset_app          | [2025-04-28 13:05:14 +0000] [34] [INFO] Worker exiting (pid: 34)
2025-04-28 18:35:15 superset_app          | [2025-04-28 13:05:15 +0000] [8] [ERROR] Worker (pid:34) exited with code 1
2025-04-28 18:35:15 superset_app          | [2025-04-28 13:05:15 +0000] [8] [ERROR] Worker (pid:34) exited with code 1.
2025-04-28 18:35:15 superset_app          | [2025-04-28 13:05:15 +0000] [59] [INFO] Booting worker with pid: 59
2025-04-28 18:35:16 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:16 superset_app          |                                     WARNING
2025-04-28 18:35:16 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:16 superset_app          | A Default SECRET_KEY was detected, please use superset_config.py to override it.
2025-04-28 18:35:16 superset_app          | Use a strong complex alphanumeric string and use a tool to help you generate 
2025-04-28 18:35:16 superset_app          | a sufficiently random sequence, ex: openssl rand -base64 42 
2025-04-28 18:35:16 superset_app          | For more info, see: https://superset.apache.org/docs/configuration/configuring-superset#specifying-a-secret_key
2025-04-28 18:35:16 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:16 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:16 superset_app          | Refusing to start due to insecure SECRET_KEY
2025-04-28 18:35:16 superset_app          | [2025-04-28 13:05:16 +0000] [59] [INFO] Worker exiting (pid: 59)
2025-04-28 18:35:17 superset_app          | [2025-04-28 13:05:17 +0000] [8] [ERROR] Worker (pid:59) exited with code 1
2025-04-28 18:35:17 superset_app          | [2025-04-28 13:05:17 +0000] [8] [ERROR] Worker (pid:59) exited with code 1.
2025-04-28 18:35:17 superset_app          | [2025-04-28 13:05:17 +0000] [84] [INFO] Booting worker with pid: 84
2025-04-28 18:35:19 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:19 superset_app          |                                     WARNING
2025-04-28 18:35:19 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:19 superset_app          | A Default SECRET_KEY was detected, please use superset_config.py to override it.
2025-04-28 18:35:19 superset_app          | Use a strong complex alphanumeric string and use a tool to help you generate 
2025-04-28 18:35:19 superset_app          | a sufficiently random sequence, ex: openssl rand -base64 42 
2025-04-28 18:35:19 superset_app          | For more info, see: https://superset.apache.org/docs/configuration/configuring-superset#specifying-a-secret_key
2025-04-28 18:35:19 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:19 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:19 superset_app          | Refusing to start due to insecure SECRET_KEY
2025-04-28 18:35:19 superset_app          | [2025-04-28 13:05:19 +0000] [84] [INFO] Worker exiting (pid: 84)
2025-04-28 18:35:19 superset_app          | [2025-04-28 13:05:19 +0000] [8] [ERROR] Worker (pid:84) exited with code 1
2025-04-28 18:35:19 superset_app          | [2025-04-28 13:05:19 +0000] [8] [ERROR] Worker (pid:84) exited with code 1.
2025-04-28 18:35:19 superset_app          | [2025-04-28 13:05:19 +0000] [109] [INFO] Booting worker with pid: 109
2025-04-28 18:35:20 superset_worker       | Loaded your LOCAL configuration at [/app/docker/pythonpath_dev/superset_config.py]
2025-04-28 18:35:20 superset_worker       | 2025-04-28 13:05:20,978:INFO:superset.initialization:Setting database isolation level to READ COMMITTED
2025-04-28 18:35:21 superset_worker_beat  | Loaded your LOCAL configuration at [/app/docker/pythonpath_dev/superset_config.py]
2025-04-28 18:35:21 superset_worker_beat  | 2025-04-28 13:05:21,113:INFO:superset.initialization:Setting database isolation level to READ COMMITTED
2025-04-28 18:35:21 superset_worker       | 2025-04-28 13:05:21,267:INFO:superset.utils.screenshots:No PIL installation found
2025-04-28 18:35:21 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:21 superset_app          |                                     WARNING
2025-04-28 18:35:21 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:21 superset_app          | A Default SECRET_KEY was detected, please use superset_config.py to override it.
2025-04-28 18:35:21 superset_app          | Use a strong complex alphanumeric string and use a tool to help you generate 
2025-04-28 18:35:21 superset_app          | a sufficiently random sequence, ex: openssl rand -base64 42 
2025-04-28 18:35:21 superset_app          | For more info, see: https://superset.apache.org/docs/configuration/configuring-superset#specifying-a-secret_key
2025-04-28 18:35:21 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:21 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:21 superset_app          | Refusing to start due to insecure SECRET_KEY
2025-04-28 18:35:21 superset_app          | [2025-04-28 13:05:21 +0000] [109] [INFO] Worker exiting (pid: 109)
2025-04-28 18:35:21 superset_worker_beat  | 2025-04-28 13:05:21,436:INFO:superset.utils.screenshots:No PIL installation found
2025-04-28 18:35:21 superset_app          | [2025-04-28 13:05:21 +0000] [8] [ERROR] Worker (pid:109) exited with code 1
2025-04-28 18:35:21 superset_app          | [2025-04-28 13:05:21 +0000] [8] [ERROR] Worker (pid:109) exited with code 1.
2025-04-28 18:35:21 superset_app          | [2025-04-28 13:05:21 +0000] [134] [INFO] Booting worker with pid: 134
2025-04-28 18:35:22 superset_worker       | 2025-04-28 13:05:22,090:INFO:superset.utils.pdf:No PIL installation found
2025-04-28 18:35:22 superset_worker_beat  | 2025-04-28 13:05:22,154:INFO:superset.utils.pdf:No PIL installation found
2025-04-28 18:35:22 superset_worker       | /app/.venv/lib/python3.11/site-packages/celery/platforms.py:829: SecurityWarning: You're running the worker with superuser privileges: this is
2025-04-28 18:35:22 superset_worker       | absolutely not recommended!
2025-04-28 18:35:22 superset_worker       | 
2025-04-28 18:35:22 superset_worker       | Please specify a different user using the --uid option.
2025-04-28 18:35:22 superset_worker       | 
2025-04-28 18:35:22 superset_worker       | User information: uid=0 euid=0 gid=0 egid=0
2025-04-28 18:35:22 superset_worker       | 
2025-04-28 18:35:22 superset_worker       |   warnings.warn(SecurityWarning(ROOT_DISCOURAGED.format(
2025-04-28 18:35:23 superset_worker_beat  | celery beat v5.4.0 (opalescent) is starting.
2025-04-28 18:35:23 superset_worker_beat  | __    -    ... __   -        _
2025-04-28 18:35:23 superset_worker_beat  | LocalTime -> 2025-04-28 13:05:23
2025-04-28 18:35:23 superset_worker_beat  | Configuration ->
2025-04-28 18:35:23 superset_worker_beat  |     . broker -> redis://redis:6379/0
2025-04-28 18:35:23 superset_worker_beat  |     . loader -> celery.loaders.app.AppLoader
2025-04-28 18:35:23 superset_worker_beat  |     . scheduler -> celery.beat.PersistentScheduler
2025-04-28 18:35:23 superset_worker_beat  |     . db -> /app/superset_home/celerybeat-schedule
2025-04-28 18:35:23 superset_worker_beat  |     . logfile -> [stderr]@%INFO
2025-04-28 18:35:23 superset_worker_beat  |     . maxinterval -> 5.00 minutes (300s)
2025-04-28 18:35:23 superset_worker_beat  | [2025-04-28 13:05:23,116: INFO/MainProcess] beat: Starting...
2025-04-28 18:35:23 superset_worker       |  
2025-04-28 18:35:23 superset_worker       |  -------------- celery@9811e3e50ee8 v5.4.0 (opalescent)
2025-04-28 18:35:23 superset_worker       | --- ***** ----- 
2025-04-28 18:35:23 superset_worker       | -- ******* ---- Linux-5.15.167.4-microsoft-standard-WSL2-x86_64-with-glibc2.36 2025-04-28 13:05:23
2025-04-28 18:35:23 superset_worker       | - *** --- * --- 
2025-04-28 18:35:23 superset_worker       | - ** ---------- [config]
2025-04-28 18:35:23 superset_worker       | - ** ---------- .> app:         __main__:0x7f0010a15810
2025-04-28 18:35:23 superset_worker       | - ** ---------- .> transport:   redis://redis:6379/0
2025-04-28 18:35:23 superset_worker       | - ** ---------- .> results:     redis://redis:6379/1
2025-04-28 18:35:23 superset_worker       | - *** --- * --- .> concurrency: 2 (prefork)
2025-04-28 18:35:23 superset_worker       | -- ******* ---- .> task events: OFF (enable -E to monitor tasks in this worker)
2025-04-28 18:35:23 superset_worker       | --- ***** ----- 
2025-04-28 18:35:23 superset_worker       |  -------------- [queues]
2025-04-28 18:35:23 superset_worker       |                 .> celery           exchange=celery(direct) key=celery
2025-04-28 18:35:23 superset_worker       |                 
2025-04-28 18:35:23 superset_worker       | 
2025-04-28 18:35:23 superset_worker       | [tasks]
2025-04-28 18:35:23 superset_worker       |   . cache-warmup
2025-04-28 18:35:23 superset_worker       |   . cache_chart_thumbnail
2025-04-28 18:35:23 superset_worker       |   . cache_dashboard_screenshot
2025-04-28 18:35:23 superset_worker       |   . cache_dashboard_thumbnail
2025-04-28 18:35:23 superset_worker       |   . fetch_url
2025-04-28 18:35:23 superset_worker       |   . prune_logs
2025-04-28 18:35:23 superset_worker       |   . prune_query
2025-04-28 18:35:23 superset_worker       |   . reports.execute
2025-04-28 18:35:23 superset_worker       |   . reports.prune_log
2025-04-28 18:35:23 superset_worker       |   . reports.scheduler
2025-04-28 18:35:23 superset_worker       |   . sql_lab.get_sql_results
2025-04-28 18:35:23 superset_worker       |   . sync_database_permissions
2025-04-28 18:35:23 superset_worker       | 
2025-04-28 18:35:23 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:23 superset_app          |                                     WARNING
2025-04-28 18:35:23 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:23 superset_app          | A Default SECRET_KEY was detected, please use superset_config.py to override it.
2025-04-28 18:35:23 superset_app          | Use a strong complex alphanumeric string and use a tool to help you generate 
2025-04-28 18:35:23 superset_app          | a sufficiently random sequence, ex: openssl rand -base64 42 
2025-04-28 18:35:23 superset_app          | For more info, see: https://superset.apache.org/docs/configuration/configuring-superset#specifying-a-secret_key
2025-04-28 18:35:23 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:23 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:23 superset_app          | Refusing to start due to insecure SECRET_KEY
2025-04-28 18:35:23 superset_app          | [2025-04-28 13:05:23 +0000] [134] [INFO] Worker exiting (pid: 134)
2025-04-28 18:35:23 superset_worker       | [2025-04-28 13:05:23,589: WARNING/MainProcess] /app/.venv/lib/python3.11/site-packages/celery/worker/consumer/consumer.py:508: CPendingDeprecationWarning: The broker_connection_retry configuration setting will no longer determine
2025-04-28 18:35:23 superset_worker       | whether broker connection retries are made during startup in Celery 6.0 and above.
2025-04-28 18:35:23 superset_worker       | If you wish to retain the existing behavior for retrying connections on startup,
2025-04-28 18:35:23 superset_worker       | you should set broker_connection_retry_on_startup to True.
2025-04-28 18:35:23 superset_worker       |   warnings.warn(
2025-04-28 18:35:23 superset_worker       | 
2025-04-28 18:35:23 superset_worker       | [2025-04-28 13:05:23,608: INFO/MainProcess] Connected to redis://redis:6379/0
2025-04-28 18:35:23 superset_worker       | [2025-04-28 13:05:23,609: WARNING/MainProcess] /app/.venv/lib/python3.11/site-packages/celery/worker/consumer/consumer.py:508: CPendingDeprecationWarning: The broker_connection_retry configuration setting will no longer determine
2025-04-28 18:35:23 superset_worker       | whether broker connection retries are made during startup in Celery 6.0 and above.
2025-04-28 18:35:23 superset_worker       | If you wish to retain the existing behavior for retrying connections on startup,
2025-04-28 18:35:23 superset_worker       | you should set broker_connection_retry_on_startup to True.
2025-04-28 18:35:23 superset_worker       |   warnings.warn(
2025-04-28 18:35:23 superset_worker       | 
2025-04-28 18:35:23 superset_worker       | [2025-04-28 13:05:23,613: INFO/MainProcess] mingle: searching for neighbors
2025-04-28 18:35:23 superset_app          | [2025-04-28 13:05:23 +0000] [8] [ERROR] Worker (pid:134) exited with code 1
2025-04-28 18:35:23 superset_app          | [2025-04-28 13:05:23 +0000] [8] [ERROR] Worker (pid:134) exited with code 1.
2025-04-28 18:35:23 superset_app          | [2025-04-28 13:05:23 +0000] [159] [INFO] Booting worker with pid: 159
2025-04-28 18:35:24 superset_worker       | [2025-04-28 13:05:24,630: INFO/MainProcess] mingle: all alone
2025-04-28 18:35:24 superset_worker       | [2025-04-28 13:05:24,655: INFO/MainProcess] celery@9811e3e50ee8 ready.
2025-04-28 18:35:25 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:25 superset_app          |                                     WARNING
2025-04-28 18:35:25 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:25 superset_app          | A Default SECRET_KEY was detected, please use superset_config.py to override it.
2025-04-28 18:35:25 superset_app          | Use a strong complex alphanumeric string and use a tool to help you generate 
2025-04-28 18:35:25 superset_app          | a sufficiently random sequence, ex: openssl rand -base64 42 
2025-04-28 18:35:25 superset_app          | For more info, see: https://superset.apache.org/docs/configuration/configuring-superset#specifying-a-secret_key
2025-04-28 18:35:25 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:25 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:25 superset_app          | Refusing to start due to insecure SECRET_KEY
2025-04-28 18:35:25 superset_app          | [2025-04-28 13:05:25 +0000] [159] [INFO] Worker exiting (pid: 159)
2025-04-28 18:35:25 superset_app          | [2025-04-28 13:05:25 +0000] [8] [ERROR] Worker (pid:159) exited with code 1
2025-04-28 18:35:25 superset_app          | [2025-04-28 13:05:25 +0000] [8] [ERROR] Worker (pid:159) exited with code 1.
2025-04-28 18:35:25 superset_app          | [2025-04-28 13:05:25 +0000] [184] [INFO] Booting worker with pid: 184
2025-04-28 18:35:26 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:26 superset_app          |                                     WARNING
2025-04-28 18:35:26 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:26 superset_app          | A Default SECRET_KEY was detected, please use superset_config.py to override it.
2025-04-28 18:35:26 superset_app          | Use a strong complex alphanumeric string and use a tool to help you generate 
2025-04-28 18:35:26 superset_app          | a sufficiently random sequence, ex: openssl rand -base64 42 
2025-04-28 18:35:26 superset_app          | For more info, see: https://superset.apache.org/docs/configuration/configuring-superset#specifying-a-secret_key
2025-04-28 18:35:26 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:26 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:26 superset_app          | Refusing to start due to insecure SECRET_KEY
2025-04-28 18:35:26 superset_app          | [2025-04-28 13:05:26 +0000] [184] [INFO] Worker exiting (pid: 184)
2025-04-28 18:35:26 superset_app          | [2025-04-28 13:05:26 +0000] [8] [ERROR] Worker (pid:184) exited with code 1
2025-04-28 18:35:26 superset_app          | [2025-04-28 13:05:26 +0000] [8] [ERROR] Worker (pid:184) exited with code 1.
2025-04-28 18:35:26 superset_app          | [2025-04-28 13:05:26 +0000] [209] [INFO] Booting worker with pid: 209
2025-04-28 18:35:27 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:27 superset_app          |                                     WARNING
2025-04-28 18:35:27 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:27 superset_app          | A Default SECRET_KEY was detected, please use superset_config.py to override it.
2025-04-28 18:35:27 superset_app          | Use a strong complex alphanumeric string and use a tool to help you generate 
2025-04-28 18:35:27 superset_app          | a sufficiently random sequence, ex: openssl rand -base64 42 
2025-04-28 18:35:27 superset_app          | For more info, see: https://superset.apache.org/docs/configuration/configuring-superset#specifying-a-secret_key
2025-04-28 18:35:27 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:27 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:27 superset_app          | Refusing to start due to insecure SECRET_KEY
2025-04-28 18:35:27 superset_app          | [2025-04-28 13:05:27 +0000] [209] [INFO] Worker exiting (pid: 209)
2025-04-28 18:35:28 superset_app          | [2025-04-28 13:05:28 +0000] [8] [ERROR] Worker (pid:209) exited with code 1
2025-04-28 18:35:28 superset_app          | [2025-04-28 13:05:28 +0000] [8] [ERROR] Worker (pid:209) exited with code 1.
2025-04-28 18:35:28 superset_app          | [2025-04-28 13:05:28 +0000] [234] [INFO] Booting worker with pid: 234
2025-04-28 18:35:29 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:29 superset_app          |                                     WARNING
2025-04-28 18:35:29 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:29 superset_app          | A Default SECRET_KEY was detected, please use superset_config.py to override it.
2025-04-28 18:35:29 superset_app          | Use a strong complex alphanumeric string and use a tool to help you generate 
2025-04-28 18:35:29 superset_app          | a sufficiently random sequence, ex: openssl rand -base64 42 
2025-04-28 18:35:29 superset_app          | For more info, see: https://superset.apache.org/docs/configuration/configuring-superset#specifying-a-secret_key
2025-04-28 18:35:29 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:29 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:29 superset_app          | Refusing to start due to insecure SECRET_KEY
2025-04-28 18:35:29 superset_app          | [2025-04-28 13:05:29 +0000] [234] [INFO] Worker exiting (pid: 234)
2025-04-28 18:35:29 superset_app          | [2025-04-28 13:05:29 +0000] [8] [ERROR] Worker (pid:234) exited with code 1
2025-04-28 18:35:29 superset_app          | [2025-04-28 13:05:29 +0000] [8] [ERROR] Worker (pid:234) exited with code 1.
2025-04-28 18:35:29 superset_app          | [2025-04-28 13:05:29 +0000] [259] [INFO] Booting worker with pid: 259
2025-04-28 18:35:31 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:31 superset_app          |                                     WARNING
2025-04-28 18:35:31 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:31 superset_app          | A Default SECRET_KEY was detected, please use superset_config.py to override it.
2025-04-28 18:35:31 superset_app          | Use a strong complex alphanumeric string and use a tool to help you generate 
2025-04-28 18:35:31 superset_app          | a sufficiently random sequence, ex: openssl rand -base64 42 
2025-04-28 18:35:31 superset_app          | For more info, see: https://superset.apache.org/docs/configuration/configuring-superset#specifying-a-secret_key
2025-04-28 18:35:31 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:31 superset_app          | --------------------------------------------------------------------------------
2025-04-28 18:35:31 superset_app          | Refusing to start due to insecure SECRET_KEY
2025-04-28 18:35:31 superset_app          | [2025-04-28 13:05:31 +0000] [259] [INFO] Worker exiting (pid: 259)
2025-04-28 18:35:31 superset_app          | [2025-04-28 13:05:31 +0000] [8] [ERROR] Worker (pid:259) exited with code 1
2025-04-28 18:35:31 superset_app          | [2025-04-28 13:05:31 +0000] [8] [ERROR] Worker (pid:259) exited with code 1.
2025-04-28 18:35:31 superset_app          | [2025-0
