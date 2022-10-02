---
id: "pr8d8"
name: Creating an Endpoint
file_version: 1.0.2
app_version: 0.9.6-0
file_blobs:
  src/sentry/api/base.py: ""
  src/sentry/api/bases/project.py: ""
  src/sentry/api/endpoints/project_platforms.py: ""
  src/sentry/api/endpoints/project_app_store_connect_credentials.py: ""
  src/sentry/plugins/bases/issue2.py: ""
  src/sentry/api/endpoints/setup_wizard.py: ""
  src/sentry/api/endpoints/project_rule_details.py: ""
  src/sentry/api/endpoints/accept_organization_invite.py: ""
  src/sentry/api/endpoints/grouping_configs.py: ""
  src/sentry/api/endpoints/project_rules.py: ""
  src/sentry/api/bases/organization.py: ""
  src/sentry/api/endpoints/organization_member_details.py: ""
---

Understanding Endpoints, how they work, and how to add new ones, is important - and this document will describe just that.

An Endpoint is {Explain what a Endpoint is and its role in the system}

Some examples of `Endpoint`[<sup id="rlVzh">↓</sup>](#f-rlVzh)s are `IssueGroupActionEndpoint`[<sup id="12BDTd">↓</sup>](#f-12BDTd), `SetupWizard`[<sup id="Z2hKmdQ">↓</sup>](#f-Z2hKmdQ), `AppStoreConnectCreateCredentialsEndpoint`[<sup id="Z1BCJlj">↓</sup>](#f-Z1BCJlj), and `ProjectRuleDetailsEndpoint`[<sup id="heFSQ">↓</sup>](#f-heFSQ).
Note: some of these examples inherit indirectly from `Endpoint`[<sup id="rlVzh">↓</sup>](#f-rlVzh).

<br/>

## Which base class to choose? 🤔

- `Endpoint`[<sup id="rlVzh">↓</sup>](#f-rlVzh): Base class of all {Explain this base class}
    - e.g. `SetupWizard`[<sup id="Z2hKmdQ">↓</sup>](#f-Z2hKmdQ), `AcceptOrganizationInvite`[<sup id="247R40">↓</sup>](#f-247R40), and `GroupingConfigsEndpoint`[<sup id="Z1shKxr">↓</sup>](#f-Z1shKxr).
- `ProjectEndpoint`[<sup id="ZcDjFw">↓</sup>](#f-ZcDjFw): Inherit from this when {Explain this base class}
    - e.g. `AppStoreConnectCreateCredentialsEndpoint`[<sup id="Z1BCJlj">↓</sup>](#f-Z1BCJlj), `ProjectRuleDetailsEndpoint`[<sup id="heFSQ">↓</sup>](#f-heFSQ), and `ProjectRulesEndpoint`[<sup id="Z770Jj">↓</sup>](#f-Z770Jj).
- `OrganizationEndpoint`[<sup id="yzWWs">↓</sup>](#f-yzWWs): Suitable base for {Explain this base class}
    - e.g. `OrganizationMemberDetailsEndpoint`[<sup id="2j1kjj">↓</sup>](#f-2j1kjj).

In this document we demonstrate inheriting from `ProjectEndpoint`[<sup id="ZcDjFw">↓</sup>](#f-ZcDjFw) as it is the most common.

<br/>

## TL;DR - How to Add a `ProjectEndpoint`[<sup id="ZcDjFw">↓</sup>](#f-ZcDjFw)

1. Create a new class inheriting from `ProjectEndpoint`[<sup id="ZcDjFw">↓</sup>](#f-ZcDjFw)&nbsp;
   - Place the file in one of the directories under `📄 src/sentry`,
     e.g. `ProjectPlatformsEndpoint`[<sup id="v80p0">↓</sup>](#f-v80p0) is defined in `📄 src/sentry/api/endpoints/project_platforms.py`.
2. Implement `get`[<sup id="ZhnPqa">↓</sup>](#f-ZhnPqa).
3. Update `📄 src/sentry/api/urls.py`.
4. **Profit** 💰

<br/>

## Example Walkthrough - `ProjectPlatformsEndpoint`[<sup id="v80p0">↓</sup>](#f-v80p0)
We'll follow the implementation of `ProjectPlatformsEndpoint`[<sup id="v80p0">↓</sup>](#f-v80p0) for this example.

A `ProjectPlatformsEndpoint`[<sup id="v80p0">↓</sup>](#f-v80p0) is {Explain what ProjectPlatformsEndpoint is and how it works with the Endpoint interface}

<br/>

### `ProjectPlatformsEndpoint`[<sup id="v80p0">↓</sup>](#f-v80p0) Usage Example
For example, this is how `ProjectPlatformsEndpoint`[<sup id="v80p0">↓</sup>](#f-v80p0) can be used:
<!-- NOTE-swimm-snippet: the lines below link your snippet to Swimm -->
### 📄 src/sentry/api/urls.py
```python
⬜ 1654                   ),
⬜ 1655                   url(
⬜ 1656                       r"^(?P<organization_slug>[^\/]+)/(?P<project_slug>[^\/]+)/platforms/$",
🟩 1657                       ProjectPlatformsEndpoint.as_view(),
⬜ 1658                       name="sentry-api-0-project-platform-details",
⬜ 1659                   ),
⬜ 1660                   url(
```

<br/>

## Steps to Adding a new `ProjectEndpoint`[<sup id="ZcDjFw">↓</sup>](#f-ZcDjFw)

<br/>

### 1\. Inherit from `ProjectEndpoint`[<sup id="ZcDjFw">↓</sup>](#f-ZcDjFw).
All `ProjectEndpoint`[<sup id="ZcDjFw">↓</sup>](#f-ZcDjFw)s are defined under `📄 src/sentry`.

<br/>

We first need to define our class in the relevant file, and inherit from `ProjectEndpoint`[<sup id="ZcDjFw">↓</sup>](#f-ZcDjFw):
<!-- NOTE-swimm-snippet: the lines below link your snippet to Swimm -->
### 📄 src/sentry/api/endpoints/project_platforms.py
```python
⬜ 5      from sentry.models import ProjectPlatform
⬜ 6      
⬜ 7      
🟩 8      class ProjectPlatformsEndpoint(ProjectEndpoint):
⬜ 9          def get(self, request, project):
⬜ 10             queryset = ProjectPlatform.objects.filter(project_id=project.id)
⬜ 11             return Response(serialize(list(queryset), request.user))
```

<br/>

### 2\. Implement `get`[<sup id="ZhnPqa">↓</sup>](#f-ZhnPqa)

Here is how we do it for `ProjectPlatformsEndpoint`[<sup id="v80p0">↓</sup>](#f-v80p0):

<br/>


<!-- NOTE-swimm-snippet: the lines below link your snippet to Swimm -->
### 📄 src/sentry/api/endpoints/project_platforms.py
```python
⬜ 6      
⬜ 7      
⬜ 8      class ProjectPlatformsEndpoint(ProjectEndpoint):
🟩 9          def get(self, request, project):
🟩 10             queryset = ProjectPlatform.objects.filter(project_id=project.id)
🟩 11             return Response(serialize(list(queryset), request.user))
⬜ 12     
```

<br/>

## Update `📄 src/sentry/api/urls.py`
Every time we add new `ProjectEndpoint`[<sup id="ZcDjFw">↓</sup>](#f-ZcDjFw)s, we reference them in `📄 src/sentry/api/urls.py`.

We will still look at `ProjectPlatformsEndpoint`[<sup id="v80p0">↓</sup>](#f-v80p0) as our example.

<br/>


<!-- NOTE-swimm-snippet: the lines below link your snippet to Swimm -->
### 📄 src/sentry/api/urls.py
```python
⬜ 1654                   ),
⬜ 1655                   url(
⬜ 1656                       r"^(?P<organization_slug>[^\/]+)/(?P<project_slug>[^\/]+)/platforms/$",
🟩 1657                       ProjectPlatformsEndpoint.as_view(),
⬜ 1658                       name="sentry-api-0-project-platform-details",
⬜ 1659                   ),
⬜ 1660                   url(
```

<br/>

## Optionally, these snippets may be helpful

<br/>

Update `📄 src/sentry/api/endpoints/debug_files.py`
<!-- NOTE-swimm-snippet: the lines below link your snippet to Swimm -->
### 📄 src/sentry/api/endpoints/debug_files.py
```python
⬜ 78         return roles.get(current_role).priority >= roles.get(required_role).priority
⬜ 79     
⬜ 80     
🟩 81     class DebugFilesEndpoint(ProjectEndpoint):
⬜ 82         permission_classes = (ProjectReleasePermission,)
⬜ 83     
⬜ 84         def download(self, debug_file_id, project):
```

<br/>

Update `📄 src/sentry/api/endpoints/project_app_store_connect_credentials.py`
<!-- NOTE-swimm-snippet: the lines below link your snippet to Swimm -->
### 📄 src/sentry/api/endpoints/project_app_store_connect_credentials.py
```python
⬜ 6          1. ``POST projects/{org_slug}/{proj_slug}/appstoreconnect/apps/``
⬜ 7      
⬜ 8          This will validate the API credentials and return the list of available applications if
🟩 9          valid, or 401 if invalid.  See :class:`AppStoreConnectAppsEndpoint`.
⬜ 10     
⬜ 11         2. ``POST projects/{org_slug}/{proj_slug}/appstoreconnect/``
⬜ 12     
```

<br/>

Update `📄 src/sentry/api/endpoints/project_app_store_connect_credentials.py`
<!-- NOTE-swimm-snippet: the lines below link your snippet to Swimm -->
### 📄 src/sentry/api/endpoints/project_app_store_connect_credentials.py
```python
⬜ 59     
⬜ 60     
⬜ 61     class AppStoreConnectCredentialsSerializer(serializers.Serializer):  # type: ignore
🟩 62         """Input validation for :class:`AppStoreConnectAppsEndpoint."""
⬜ 63     
⬜ 64         # an IID with the XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX format
⬜ 65         appconnectIssuer = serializers.CharField(max_length=36, min_length=36, required=False)
```

<br/>

Update `📄 src/sentry/api/endpoints/project_app_store_connect_credentials.py`
<!-- NOTE-swimm-snippet: the lines below link your snippet to Swimm -->
### 📄 src/sentry/api/endpoints/project_app_store_connect_credentials.py
```python
⬜ 71         id = serializers.CharField(max_length=40, min_length=1, required=False)
⬜ 72     
⬜ 73     
🟩 74     class AppStoreConnectAppsEndpoint(ProjectEndpoint):  # type: ignore
⬜ 75         """Retrieves available applications with provided credentials.
⬜ 76     
⬜ 77         ``POST projects/{org_slug}/{proj_slug}/appstoreconnect/apps/``
```

<br/>

Update `📄 src/sentry/api/endpoints/project_processingissues.py`
<!-- NOTE-swimm-snippet: the lines below link your snippet to Swimm -->
### 📄 src/sentry/api/endpoints/project_processingissues.py
```python
⬜ 9      from sentry.web.helpers import render_to_response
⬜ 10     
⬜ 11     
🟩 12     class ProjectProcessingIssuesDiscardEndpoint(ProjectEndpoint):
⬜ 13         def delete(self, request, project):
⬜ 14             """
⬜ 15             This discards all open processing issues
```

<br/>

<!-- THIS IS AN AUTOGENERATED SECTION. DO NOT EDIT THIS SECTION DIRECTLY -->
### Swimm Note

<span id="f-247R40">AcceptOrganizationInvite</span>[^](#247R40) - "src/sentry/api/endpoints/accept_organization_invite.py" L11
```python
class AcceptOrganizationInvite(Endpoint):
```

<span id="f-Z1BCJlj">AppStoreConnectCreateCredentialsEndpoint</span>[^](#Z1BCJlj) - "src/sentry/api/endpoints/project_app_store_connect_credentials.py" L188
```python
class AppStoreConnectCreateCredentialsEndpoint(ProjectEndpoint):  # type: ignore
```

<span id="f-rlVzh">Endpoint</span>[^](#rlVzh) - "src/sentry/api/base.py" L95
```python
class Endpoint(APIView):
```

<span id="f-ZhnPqa">get</span>[^](#ZhnPqa) - "src/sentry/api/endpoints/project_platforms.py" L9
```python
    def get(self, request, project):
```

<span id="f-Z1shKxr">GroupingConfigsEndpoint</span>[^](#Z1shKxr) - "src/sentry/api/endpoints/grouping_configs.py" L8
```python
class GroupingConfigsEndpoint(Endpoint):
```

<span id="f-12BDTd">IssueGroupActionEndpoint</span>[^](#12BDTd) - "src/sentry/plugins/bases/issue2.py" L23
```python
class IssueGroupActionEndpoint(PluginGroupEndpoint):
```

<span id="f-yzWWs">OrganizationEndpoint</span>[^](#yzWWs) - "src/sentry/api/bases/organization.py" L158
```python
class OrganizationEndpoint(Endpoint):
```

<span id="f-2j1kjj">OrganizationMemberDetailsEndpoint</span>[^](#2j1kjj) - "src/sentry/api/endpoints/organization_member_details.py" L76
```python
class OrganizationMemberDetailsEndpoint(OrganizationEndpoint):
```

<span id="f-ZcDjFw">ProjectEndpoint</span>[^](#ZcDjFw) - "src/sentry/api/bases/project.py" L123
```python
class ProjectEndpoint(Endpoint):
```

<span id="f-v80p0">ProjectPlatformsEndpoint</span>[^](#v80p0) - "src/sentry/api/endpoints/project_platforms.py" L8
```python
class ProjectPlatformsEndpoint(ProjectEndpoint):
```

<span id="f-heFSQ">ProjectRuleDetailsEndpoint</span>[^](#heFSQ) - "src/sentry/api/endpoints/project_rule_details.py" L23
```python
class ProjectRuleDetailsEndpoint(ProjectEndpoint):
```

<span id="f-Z770Jj">ProjectRulesEndpoint</span>[^](#Z770Jj) - "src/sentry/api/endpoints/project_rules.py" L48
```python
class ProjectRulesEndpoint(ProjectEndpoint):
```

<span id="f-Z2hKmdQ">SetupWizard</span>[^](#Z2hKmdQ) - "src/sentry/api/endpoints/setup_wizard.py" L16
```python
class SetupWizard(Endpoint):
```

<br/>

This file was generated by Swimm. [Click here to view it in the app](https://swimm-web-app.web.app/repos/testRepoId/docs/).
