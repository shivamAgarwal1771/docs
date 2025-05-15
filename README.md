Dockerfile:214
--------------------
 212 |     COPY superset superset
 213 |     # TODO in the meantime, remove the .po files
 214 | >>> RUN rm superset/translations/*/*/*.po
 215 |
 216 |     # Merging translations from backend and frontend stages
--------------------
ERROR: failed to solve: process "/bin/sh -c rm superset/translations/*/*/*.po" did not complete successfully: exit code: 1
