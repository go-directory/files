# schema

The schema subdirectory contains myriad schema definitions, each derived from various RFCs, ITU-T recommendations and (unofficially accepted) Internet-Drafts. Such definitions shall henceforth be referred to as ***official definitions***.

Each category of definition is organized within dedicated subdirectories, each uniquely named and specially ordered. All schema files MUST bear the `.schema` extension. The [schema parser](https://github.com/go-directory/schema) will ignore files which do NOT bear this extension during its traversal of the schema directory structure.

## Official definitions

***Official definitions*** are organized as follows:

  - `00-ls` _LDAP syntaxes_
  - `01-mr` _matching rules_
  - `02-at` _attribute types_
  - `03-oc` _object classes_
  - `04-dc` _DIT content rules_
  - `05-nf` _name forms_
  - `06-ds` _DIT structure rules_

Within some of these directories are files such as `03-rfc4512.schema`. In each of these files are definitions derived from the named source.

## Go Directory definitions

Within the `50-gd` subdirectory are schema files created and maintained by the [Go Directory Project](https://github.com/go-directory).  Among them are the following files:

  - `00-config.schema` _cn=config runtime configuration_
  - `01-monitor.schema` _cn=monitor_
  - `02-state.schema` _cn=state_
  - `03-meta.schema` _ou=schema (meta schema)_

## User definitions

The `90-user` subdirectory is _always_ empty in this repository. The subdirectory exists merely to advertise a location for the deployment of custom schemata on the DSA(s) in question.

Directory administrators MUST ensure that custom `.schema` files are ordered in terms of dependencies, else the schema parser will return an error.

# How to deploy

 - Clone this repository
 - Move or copy the `schema` subdirectory to the desired location on disk intended for the storage of schema files (e.g.: `/usr/local/go-directory/etc/schema`)
   - If you wish, you can discard anything else present within the clone that was not in the `schema` directory
   - The DSA does not need write privileges to schema content on disk, therefore the octal mode of the _directories_ may be read and execute only, and the `.schema` _files_ may be read only
 - In your `dse.ldif` file, and within the `cn=schemata,cn=config` entry, set the `schemaDirectoryPath` attribute to the full path chosen for your schema root above
 - Start the DSA, watch for any schema errors
