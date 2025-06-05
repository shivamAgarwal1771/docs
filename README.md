 => ERROR [superset-init python-base 5/6] RUN pip install --no-cache-dir --upgrade uv                                          11.7s
------
 > [superset-init python-base 5/6] RUN pip install --no-cache-dir --upgrade uv:
3.709 WARNING: Retrying (Retry(total=4, connect=None, read=None, redirect=None, status=None)) after connection broken by 'ProtocolError('Connection aborted.', ConnectionResetError(104, 'Connection reset by peer'))': /simple/uv/
4.280 WARNING: Retrying (Retry(total=3, connect=None, read=None, redirect=None, status=None)) after connection broken by 'ProtocolError('Connection aborted.', ConnectionResetError(104, 'Connection reset by peer'))': /simple/uv/
5.335 WARNING: Retrying (Retry(total=2, connect=None, read=None, redirect=None, status=None)) after connection broken by 'ProtocolError('Connection aborted.', ConnectionResetError(104, 'Connection reset by peer'))': /simple/uv/
7.393 WARNING: Retrying (Retry(total=1, connect=None, read=None, redirect=None, status=None)) after connection broken by 'ProtocolError('Connection aborted.', ConnectionResetError(104, 'Connection reset by peer'))': /simple/uv/
11.44 WARNING: Retrying (Retry(total=0, connect=None, read=None, redirect=None, status=None)) after connection broken by 'ProtocolError('Connection aborted.', ConnectionResetError(104, 'Connection reset by peer'))': /simple/uv/
11.48 ERROR: Could not find a version that satisfies the requirement uv (from versions: none)
11.48 ERROR: No matching distribution found for uv
------
failed to solve: process "/bin/sh -c pip install --no-cache-dir --upgrade uv" did not complete successfully: exit code: 1
