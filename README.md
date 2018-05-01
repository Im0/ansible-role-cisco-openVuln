Cisco openVuln API Ansible role
===============================

This role queries the Cisco Product Security Incident Response Team (PSIRT) openVuln API for useful information regarding security advisories.

Currently can query:
* IOS/IOS XE software endpoint
* Hello API (Hello World)

More information available here:
* https://developer.cisco.com/site/PSIRT/discover/overview/
* https://developer.cisco.com/site/PSIRT/get-started/getting-started.html (future API endpoints to support)

These Cisco API's support the [CVRF](https://communities.cisco.com/docs/DOC-63679) and [OVAL](https://communities.cisco.com/docs/DOC-63158) standards.


Future/TODO
-----------

Initially very basic, this role in the future will hopefully cater for the majority of the endpoints and options as specified in the Cisco [openVuln Swagger](https://github.com/CiscoPSIRT/openVulnAPI/blob/master/swagger/openVulnAPISwagger0_0_4.yaml) definition.

In addition to this, depending on interest:
* Report generation
* Sorting by CVSS score
* Caching of results (so we don't hit the API too much)


Requirements
------------

* A Cisco CCO ID
* Register an application on the Cisco API Console (https://apiconsole.cisco.com/)
> My Application > New Application > `Any name` and select `Client Credentials`
* Retrieve the API client id and client secrets for the API you wish to query.  This role uses `Cisco PSIRT openVuln API` and `Hello API`, ensure you enable those APIs in the console.

Role Variables
--------------

**Required:**
`openVuln_client_id`: Used as part of oauth for the openVuln API
`openVuln_client_secret`: Used as part of oauth for the openVuln API 
`lookup`: The type of lookup to make.  Options are:  [ `hello` | `ios` | `iosxe` ]

`version`: Version is a **requirement** for `lookup` types `ios` and `iosxe`.

**Optional**
`helloAPI_client_id`: Used as part of oauth for the helloAPI
`helloAPI_client_secret`: Used as part of oauth for the helloAPI


Return Data
-----------

| Lookup      | Registered return variable |
|-------------|----------------------------|
| ios         | openVuln_ios_version       |
| iosxe       | openVuln_iosxe_version     |
| hello       | hello                      |


Dependencies
------------

None.

Example Playbook
----------------


    - hosts: localhost
      vars:
          helloAPI_client_id: *replace me*
          helloAPI_client_secret: *replace me*
          openVuln_client_id: *replace me*
          openVuln_client_secret: *replace me*
      roles:
         - { role: Im0.ansible-role-cisco-openVuln, 
             lookup: 'ios',
             version: '15.2(5)e' }
         - { role: Im0.ansible-role-cisco-openVuln,
             lookup: 'hello' }
      tasks:
         - name: Run ios_version instead of 'main'
            include_role:
              name: ansible-role-cisco-openVuln
              tasks_from: ios_version
            vars:
              lookup: 'ios'
              version: '15.2(5)e'
          - debug:
              var: openVuln_ios_version


**Returns:**
Variable `openVuln_ios_version` is returned from the Ansible URI module.  The JSON return data is available within `openVuln_ios_version.json`.
Variable `hello` is returned from the Ansible URI module.  The JSON return data is available within `hello.json`.


License
-------

GNU General Public License v3.0

See [COPYING](https://github.com/ansible/ansible/blob/devel/COPYING) to see the full text.

Author Information
------------------

John Imison <john+github@imison.net> 
