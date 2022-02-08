=====
Usage
=====

To use Atlas API SDK in a project:

.. code-block:: python

    from mongodb_atlas_api_sdk.mongodb_atlas_api_sdk import *


Manage Database Users
===========================

.. code-block:: python

    from mongodb_atlas_api_sdk.mongodb_atlas_api_sdk import Atlas, AtlasProject, DatabaseUser

    atlas_conn = Atlas(public_key, private_key)
    project = AtlasProject(atlas_conn, project_id)
    print(project.users)

Add new AWS user
------------------

.. code-block:: python

    from mongodb_atlas_api_sdk.mongodb_atlas_api_sdk import Atlas, AtlasProject, DatabaseUser

    atlas_conn = Atlas(public_key, private_key)
    project = AtlasProject(atlas_conn, project_id)

    user = DatabaseUser(atlas, project, "arn:aws:iam::123456789012:role/role-name", aws_iam_type="ROLE")
    user.add_role(name="readWriteAnyDatabase")
    if not user.exists:
        user.create()


Add new SCRAM user
-------------------

.. code-block:: python

    from mongodb_atlas_api_sdk.mongodb_atlas_api_sdk import Atlas, AtlasProject, DatabaseUser

    atlas_conn = Atlas(public_key, private_key)
    project = AtlasProject(atlas_conn, project_id)

    user = DatabaseUser(atlas, project, "newuser_name", password="ASecretPassword")
    user.add_role(name="readWriteAnyDatabase")
    if not user.exists:
        user.create()


Update existing user
------------------------

.. code-block:: python

    from mongodb_atlas_api_sdk.mongodb_atlas_api_sdk import Atlas, AtlasProject, DatabaseUser

    atlas_conn = Atlas(public_key, private_key)
    project = AtlasProject(atlas_conn, project_id)

    user = DatabaseUser(atlas, project, "arn:aws:iam::123456789012:role/role-name", aws_iam_type="ROLE")
    user.add_role(name="readWriteAnyDatabase")
    if user.exists:
        user.set_from_api_describe(**user.read())
    print(user.roles)
    user.add_role("readWrite", "newDB", "mycollection/*")
    user.update()

Delete user
------------------

.. code-block:: python

    from mongodb_atlas_api_sdk.mongodb_atlas_api_sdk import Atlas, AtlasProject, DatabaseUser

    atlas_conn = Atlas(public_key, private_key)
    project = AtlasProject(atlas_conn, project_id)

    user = DatabaseUser(atlas, project, "arn:aws:iam::123456789012:role/role-name", aws_iam_type="ROLE")
    if user.exists:
        user.set_from_api_describe(**user.read())
    user.delete()
