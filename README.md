orasash
=======

Oracle Simulation ASH

Project home page - http://pioro.github.io/orasash/

Change log:


Change Log:

2.3.1
repo_schema.sql
- new column in sashX table - osuser varchar2(30)
- new column in sash_event_names - event_id  number
- change in v$active_session_history view definition for following columns qc_instance_id,qc_session_id,qc_session_serial#

sash_pkg.sql
- changes related to new columns

sash_repo.sql
- using DBID as a part of job name
- support for same database name on different hosts

installation script
- fix of hardcoded sash schema
- some other minor fixes


If you want to keep old data with new version of sash you need to do all 3 changes from repo_schema.sql manually.



2.4-rc1

This version of S-ASH is collecting event histograms, OS statistics from
Oracle and sys time model.

v$active_session_history is still a main view but the following AWR views are now simulated:

- dba_hist_snapshot
- dba_hist_sysmetric_history
- dba_hist_sqlstat
- dba_hist_active_sess_history
- dba_hist_event_histogram
- dba_hist_sys_time_model


File changes:

sash_xplan.sql
- fix for duplicated sql_id / plan_hash for RAC monitoring
- view SASH_PLAN_TABLE moved from repo_schema.sql

sash_pkg.sql
- fix for hostname with dash
- collect_osstat - new procedure to collect v$osstat
- collect_sys_time_model - new procedure to collect v$sys_time_model
- collect_event_histogram - new procedure to collect v$event_histogram

sash_repo.sql
- fix for hostname with dash


repo_schema.sql
- add inst_num column to sash_target table
- all views are using dbid and inst_num - so if you want to consolidate
  results from RAC use raw sash tables
- split views to other file
- sash_sqlstats and sash_event_histogram have a "poor man" paritioning


switchdb.sql / current.sql
- instance switch added


upgarde: TODO from 2.3.1 to 2.4 without data loss

Number of metric changed - steps to run if you upgraded from lower version

PROD_SASH@XE  > delete from sash_sysmetric_names where dbid = <dbid>;

21 rows deleted.

PROD_SASH@XE  > commit;

Commit complete.

PROD_SASH@XE  > exec sash_pkg.get_metrics('<db_link>')

PL/SQL procedure successfully completed.

PROD_SASH@XE  > select * from sash_sysmetric_names where dbid = <dbid>;

...


Marcin
