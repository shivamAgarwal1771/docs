
2025-05-06 08:55:24,655:WARNING:superset.commands.dataset.importers.v1.utils:Loading data outside the import transaction

/app/superset/connectors/sqla/models.py:2069: SAWarning: Usage of the 'related attribute set' operation is not currently supported within the execution stage of the flush process. Results may not be consistent.  Consider using alternative event listeners or connection-level operations instead.

  self.database = session.query(Database).filter_by(id=self.database_id).one()

/app/superset/connectors/sqla/models.py:2069: SAWarning: Usage of the 'collection append' operation is not currently supported within the execution stage of the flush process. Results may not be consistent.  Consider using alternative event listeners or connection-level operations instead.

  self.database = session.query(Database).filter_by(id=self.database_id).one()

/app/superset/models/helpers.py:323: SAWarning: Attribute history events accumulated on 1 previously clean instances within inner-flush event handlers have been reset, and will not result in database updates. Consider using set_committed_value() within inner-flush event handlers to avoid this warning.

  obj = obj_query.one_or_none()

2025-05-06 08:55:24,786:INFO:superset.commands.dataset.importers.v1.utils:Downloading data from https://raw.githubusercontent.com/apache-superset/examples-data/master/datasets/examples/slack/threads.csv⁠

2025-05-06 08:55:29,082:WARNING:superset.commands.dataset.importers.v1.utils:Loading data outside the import transaction

/app/superset/connectors/sqla/models.py:2069: SAWarning: Usage of the 'related attribute set' operation is not currently supported within the execution stage of the flush process. Results may not be consistent.  Consider using alternative event listeners or connection-level operations instead.

  self.database = session.query(Database).filter_by(id=self.database_id).one()

/app/superset/connectors/sqla/models.py:2069: SAWarning: Usage of the 'collection append' operation is not currently supported within the execution stage of the flush process. Results may not be consistent.  Consider using alternative event listeners or connection-level operations instead.

  self.database = session.query(Database).filter_by(id=self.database_id).one()

/app/superset/models/helpers.py:323: SAWarning: Attribute history events accumulated on 1 previously clean instances within inner-flush event handlers have been reset, and will not result in database updates. Consider using set_committed_value() within inner-flush event handlers to avoid this warning.

  obj = obj_query.one_or_none()
