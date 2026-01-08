===========
aws-mfa-env
===========

* Author: `Adam Bolte`_
* Contact: adam.bolte@sitepoint.com

.. _`Adam Bolte`: https://www.sitepoint.com/author/adam-bolte/

This is a small Bash script that gives the user the ability to quickly
obtain AWS session credentials as shell environment variables, using
heavily restricted IAM access keys and a MFA device.

For example, a user might have a policy applied to deny nearly any
service access until the user has enabled MFA, as described by Amazon
`here
<https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_examples_aws_my-sec-creds-self-manage-mfa-only.html>`__.


Project page
------------

https://github.com/sitepoint/aws-mfa-env


Requirements
============

* `AWS Command Line Interface version 2`_
* `GNU Bash`_
* `GNU grep`_
* `jq`_

.. _AWS Command Line Interface version 2: https://aws.amazon.com/cli/
.. _GNU Bash: http://www.gnu.org/software/bash/
.. _GNU grep: https://www.gnu.org/software/grep/
.. _jq: https://jqlang.github.io/jq/

These are the dependencies that were tested, and all with the possible
exception of the AWS Command Line Interface (AWS CLI) should be
available in almost any desktop or server GNU/Linux distribution's
package management system.


Setup
=====

Place the aws-mfa-env script somewhere in your path. It need not have
executable permissions set. Next, in your login shell
(eg. ``~/.bashrc`` for GNU Bash, ``~/.zshrc`` for Z shell) file add
the following line:

.. code-block:: console

  . aws-mfa-env


Usage
=====

Ensure ``AWS_ACCESS_KEY_ID`` and ``AWS_SECRET_ACCESS_KEY`` environment
variables are set or, alternatively, use ``AWS_PROFILE`` to point to a
profile that has these — ideally configured in ``~/.aws/config`` and
not ``~/.aws/credentials`` (see the note about
``AWS_MFA_ENV_WRITE_CREDENTIALS`` below).

In addition, set ``AWS_MFA_ARN`` to the ARN of your IAM account's MFA
device. It will take this format:

``arn:aws:iam::<AWS ACCOUNT>:mfa/<IAM USER>``

Once those have been taken care of, simply run the ``aws-mfa-env``
command.

.. code-block:: console

  $ aws-mfa-env 
  MFA token: 123456
  Success!
  Expiration: "2024-07-25T19:13:43+00:00"
  $

This will set new values for the environment variables
``AWS_ACCESS_KEY_ID`` and ``AWS_SECRET_ACCESS_KEY``, and also export a
new environment variable ``AWS_SESSION_TOKEN``. With these in place,
you can now execute other commands from the AWS Command Line Interface
(aka. aws-cli) or scripts that use libraries provided by AWS (such as
Python scripts that use boto3), provided you have the appropriate
permissions to do so.

At no point does aws-mfa-env write the session credentials to a file,
unless requested by setting ``AWS_MFA_ENV_WRITE_CREDENTIALS=1`` (in
which case it will overwrite ``~/.aws/credentials`` — use with
caution!).


Assume a role
=============

If permissions are assigned to a role that the user must assume,
aws-mfa-env will attempt to take care of this automatically if
``AME_AWS_ROLE_ARN`` (and optionally ``AME_AWS_ROLE_SESSION_NAME``)
are set. The values for these should be set to the `AWS equivalents
<https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html>`__
(that have the same name minus the ``AME_`` prefix).



Issues
======

If you encounter any bugs or would like to propose a new feature, feel
free to open an issue on GitHub, however please be patent with a
response.

Likewise, pull requests are also welcome.



Related
=======

If you like this project, you might also find `envswitch`_
useful. These are both stand-alone tools but work very well together.

.. _envswitch: https://github.com/sitepoint/envswitch
