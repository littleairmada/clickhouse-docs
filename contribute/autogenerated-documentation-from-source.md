# Generating documentation from source code

A [script in our docs repo](https://github.com/ClickHouse/clickhouse-docs/blob/main/scripts/settings/autogenerate-settings.sh) 
is used to run SQL queries which generate markdown documentation from the system tables in ClickHouse and insert it 
into the appropriate pages before build time of the docs.

## Session settings

Documentation for session settings is autogenerated from source file [`src/Core/Settings.cpp`](https://github.com/ClickHouse/ClickHouse/blob/master/src/Core/Settings.cpp).
[SQL](../scripts/settings/session-settings.sql)

## Format settings

Documentation for session settings is autogenerated from source file [`src/Core/FormatFactorySettings.h`](https://github.com/ClickHouse/ClickHouse/blob/master/src/Core/FormatFactorySettings.h).
[SQL](../scripts/settings/format-settings.sql)

## Server settings

Documentation for session settings are extracted from [`src/Core/ServerSettings.cpp`](https://github.com/ClickHouse/ClickHouse/blob/master/src/Core/ServerSettings.cpp).

Note that this file contains only a fraction of all server settings. 
The reason is that server settings can be nested and `system.server_settings` 
(which is built from `src/Core/ServerSettings.cpp`) and cannot represent nested
settings.

Example:

```yaml
<query_cache>
    <max_size_in_bytes>1073741824</max_size_in_bytes>
    <max_entries>1024</max_entries>
    <max_entry_size_in_bytes>1048576</max_entry_size_in_bytes>
    <max_entry_size_in_rows>30000000</max_entry_size_in_rows>
</query_cache>
```
As a result of the above, you will find the server settings which are not found 
in `system.server_settings` documented in file `_server_settings_outside_source.md`

The auto-generation script reads these in, combines them with the ones from 
`system.settings` and appends the formatted settings to `settings.md`.

**As such, if you need to make a change to the settings you see on the 
[server settings](clickhouse.com/docs/operations/server-configuration-parameters/) 
page, you will need to check if the setting is in `system.server_settings`. 
If it is, then please edit the setting description in the source code documentation
in [`ServerSettings.cpp`](https://github.com/ClickHouse/ClickHouse/blob/master/src/Core/ServerSettings.cpp) 
or else edit `_server_settings_outside_source.md`.**

[SQL](../scripts/settings/global-server-settings.sql)

## MergeTree settings

Documentation for MergeTree settings is autogenerated from [MergeTreeSettings.cpp](https://github.com/ClickHouse/ClickHouse/blob/master/src/Storages/MergeTree/MergeTreeSettings.cpp)

[SQL](../scripts/settings/mergetree-settings.sql)

## System tables

TO DO

## Functions

Documentation for functions is autogenerated from the documentation in system tables (`system.functions`) which is 
registered along with the function registration in the C++ code.

To add a new page:
- add SQL to generate the markdown from system tables
- update `autogenerate-settings.sh` in the section which specifies which markdown file contents to
  copy to which file in between the `<!--AUTOGENERATED_START-->` and `<!--AUTOGENERATED_END-->`

The following pages are autogenerated from the source code for functions:
- [Arithmetic](/sql-reference/functions/arithmetic-functions) ([SQL](../scripts/settings/arithmetic-functions.sql))
- [Arrays](/sql-reference/functions/array-functions) ([SQL](../scripts/settings/array-functions.sql))
- [Bit](/sql-reference/functions/bit-functions) ([SQL](../scripts/settings/bit-functions.sql))
- [Bitmap](/sql-reference/functions/bitmap-functions) ([SQL](../scripts/settings/bitmap-functions.sql))
- [Comparison](/sql-reference/functions/comparison-functions) ([SQL](../scripts/settings/comparison-functions.sql))
- [Conditional](/sql-reference/functions/conditional-functions) ([SQL](../scripts/settings/conditional-functions.sql))

