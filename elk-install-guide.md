# Elastic Stack Installation

1. Download and extrat the following software packages
   1. Elastisearch: https://www.elastic.co/downloads/elasticsearch
   2. Kibana: https://www.elastic.co/downloads/kibana
   3. Filebeat: https://www.elastic.co/downloads/beats/filebeat

2. In ElasticSearch install directory:
    * `$> bin/elasticsearch-plugin install x-pack`
    * `$> bin/elasticsearch`
    * Open a new terminal window to generate elastic passwords
    *  `$> bin/x-pack/setup-passwords auto`
        * Note the password for elastic user as `<es_pw>`
        * Note the password for kibana user as `<kibana_pw>`

3. In Kibana install directory:
    * `$> bin/kibana-plugin install x-pack`
    * Modify `config/kibana.yml` to set credentials for Elasticsearch output
        >     elasticsearch.username: "kibana"  
        >     elasticsearch.password: "<kibana_pw>"
    * Execute kibana with: `$> bin/kibana`

4. In Filebeat directory and modify `filebeat.yml` 
    * To set credentials for Elastiserach output
        >     output.elasticsearch:  
        >       username: "elastic"  
        >       password: "<es_pw>"
    * To adjust the path log file, include the following snippet (if it do not exists)
    ```yml 
     #=========================== Filebeat prospectors =============================

     filebeat.prospectors:

     # Each - is a prospector. Most options can be set at the prospector level, so
     # you can use different prospectors for various configurations.
     # Below are the prospector specific configurations.

     - type: log

     # Change to true to enable this prospector configuration.
      enabled: true

     # Paths that should be crawled and fetched. Glob based paths.
     paths:
        - /var/log/middleware/*.log #/var/log/*.log
        #- c:\programdata\elasticsearch\logs\*

     # Exclude lines. A list of regular expressions to match. It drops the lines that are
     # matching any regular expression from the list.
     #exclude_lines: ['^DBG']

     # Include lines. A list of regular expressions to match. It exports the lines that are
     # matching any regular expression from the list.
     #include_lines: ['^ERR', '^WARN']

     # Exclude files. A list of regular expressions to match. Filebeat drops the files that
     # are matching any regular expression from the list. By default, no files are dropped.
     #exclude_files: ['.gz$']

     # Optional additional fields. These fields can be freely picked
     # to add additional information to the crawled log files for filtering
     #fields:
     #  level: debug
     #  review: 1

     ### Multiline options

     # Mutiline can be used for log messages spanning multiple lines. This is common
     # for Java Stack Traces or C-Line Continuation

     # The regexp Pattern that has to be matched. The example pattern matches all lines starting with [
     multiline.pattern: '^[A-Z]+[[:space:]]+\[[0-9]{4}-[0-9]{2}-[0-9]{2}'

     # Defines if the pattern set under pattern should be negated or not. Default is false.
     multiline.negate: true

     # Match can be set to "after" or "before". It is used to define if lines should be append to a pattern
     # that was (not) matched before or after or as long as a pattern is not matched based on negate.
     # Note: After is the equivalent to previous and before is the equivalent to to next in Logstash
     multiline.match: after
    ```

   


