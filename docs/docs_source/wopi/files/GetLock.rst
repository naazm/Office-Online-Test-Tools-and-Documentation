
..  index:: WOPI requests; GetLock, GetLock

..  |operation| replace:: GetLock

..  _GetLock:

GetLock
=======

..  default-domain:: http

..  post:: /wopi*/files/(file_id)

    ..  warning::
        This operation is not yet called by Office Online. It has been added to the WOPI protocol definition, and
        Office Online will call it in the future, but it does not currently.

    The |operation| operation retrieves a lock on a file. Note that this operation *does not create a new lock.* Rather,
    this operation always returns the current lock value in the **X-WOPI-Lock** response header. Because of this, its
    semantics differ slightly from the other lock-related operations.

    If the file is currently not locked, the host must return a :http:statuscode:`200` and include an **X-WOPI-Lock**
    response header set to the empty string.

    If the file is currently locked, the host must return either a :http:statuscode:`200` or a
    :http:statuscode:`409`. In both cases, the host must include an **X-WOPI-Lock** response header containing the
    value of the current lock on the file. Note that hosts must respond with a :http:statuscode:`409` if they wish to
    include an **X-WOPI-LockFailureReason** or **X-WOPI-LockedByOtherInterface** response header.

    ..  include:: /fragments/no_lock_id.rst

    See :term:`Lock` for more general information regarding locks.


    ..  include:: /fragments/common_params.rst

    :reqheader X-WOPI-Override:
        The **string** ``GET_LOCK``. Required.

    :resheader X-WOPI-Lock:
        A **string** value identifying the current lock on the file. Unlike other lock operations, this header is
        required when responding to the request with either :http:statuscode:`200` or :http:statuscode:`409`.

    :resheader X-WOPI-LockFailureReason:
        An optional **string** value indicating the cause of a lock failure. This header may be included when
        responding to the request with :http:statuscode:`409`. There is no standard for how this string is
        formatted, and Office Online only uses it for logging purposes. However, we recommend hosts use small strings
        that are consistent. This allows Office Online to easily report to hosts how often locks are failing due to
        particular reasons.

    :resheader X-WOPI-LockedByOtherInterface:
        An optional **string** value indicating that the file is currently locked by someone other than Office Online.
        This header is optional, and is only used by Office Online to provide more specific messages to users when
        operations fail due to locks. If set, the value of this header must be the string ``true``.


    :code 200: Success; an **X-WOPI-Lock** response header containing the value of the current lock on the file must
        always be included when using this response code
    :code 401: Invalid :term:`access token`
    :code 404: File unknown/user unauthorized
    :code 409: Lock mismatch/locked by another interface; an **X-WOPI-Lock** response header containing the value of
        the current lock on the file must always be included when using this response code
    :code 500: Server error
    :code 501: Unsupported

    ..  include:: /fragments/common_headers.rst
