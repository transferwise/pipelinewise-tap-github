1.0.2 (2021-08-13)
------------------
- Skipping repos with size 0 on list all repos
- Fixing wrong order of include/exclude params

1.0.1 (2021-08-09)
------------------
- Do not do rate throtting during discovery

1.0.0 (2021-07-19)
------------------

- This is a fork of https://github.com/singer-io/tap-github v1.10.0. 
- Add `organization`, `repos_include`, `repos_exclude`, `include_archived` and `include_disabled` options.
- Add `max_rate_limit_wait_seconds` option to wait if you hit the github api limit.
