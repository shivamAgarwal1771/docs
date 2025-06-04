 => ERROR [superset-init python-base 5/6] RUN pip install --no-cache-dir --upgrade uv                                          11.6s
------
 > [superset-init python-base 5/6] RUN pip install --no-cache-dir --upgrade uv:
3.572 WARNING: Retrying (Retry(total=4, connect=None, read=None, redirect=None, status=None)) after connection broken by 'ProtocolError('Connection aborted.', ConnectionResetError(104, 'Connection reset by peer'))': /simple/uv/
4.136 WARNING: Retrying (Retry(total=3, connect=None, read=None, redirect=None, status=None)) after connection broken by 'ProtocolError('Connection aborted.', ConnectionResetError(104, 'Connection reset by peer'))': /simple/uv/
5.177 WARNING: Retrying (Retry(total=2, connect=None, read=None, redirect=None, status=None)) after connection broken by 'ProtocolError('Connection aborted.', ConnectionResetError(104, 'Connection reset by peer'))': /simple/uv/
7.228 WARNING: Retrying (Retry(total=1, connect=None, read=None, redirect=None, status=None)) after connection broken by 'ProtocolError('Connection aborted.', ConnectionResetError(104, 'Connection reset by peer'))': /simple/uv/
11.27 WARNING: Retrying (Retry(total=0, connect=None, read=None, redirect=None, status=None)) after connection broken by 'ProtocolError('Connection aborted.', ConnectionResetError(104, 'Connection reset by peer'))': /simple/uv/
11.32 ERROR: Could not find a version that satisfies the requirement uv (from versions: none)
11.32 ERROR: No matching distribution found for uv
------
failed to solve: process "/bin/sh -c pip install --no-cache-dir --upgrade uv" did not complete successfully: exit code: 1
