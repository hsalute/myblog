# myblog

----------------- //telegraf set up ---test arista gnmi
 [[outputs.influxdb_v2]]
  ## The URLs of the InfluxDB cluster nodes.
  ##
  ## Multiple URLs can be specified for a single cluster, only ONE of the
  ## urls will be written to each interval.
  ##   ex: urls = ["https://us-west-2-1.aws.cloud2.influxdata.com"]
  urls = ["http://x.x.x.x:8086"]

  ## API token for authentication.
  token = "0MyZWaVdTYe_xb-tC1uVBniogW_echPzDrUC2vTn9PTHfqXci-NgPgJhb0x4zVJTTviBjxduAwsMxZKx172-mQ=="

  ## Organization is the name of the organization you wish to write to; must exist.
  organization = "influxdb"

  ## Destination bucket to write into.
  bucket = "arista"

  ## The value of this tag will be used to determine the bucket.  If this
  ## tag is not set the 'bucket' option is used as the default.
  # bucket_tag = ""

  ## If true, the bucket tag will not be added to the metric.
  # exclude_bucket_tag = false

  ## Timeout for HTTP messages.
  # timeout = "5s"

  ## Additional HTTP headers
  # http_headers = {"X-Special-Header" = "Special-Value"}

  ## HTTP Proxy override, if unset values the standard proxy environment
  ## variables are consulted to determine which proxy, if any, should be used.
  # http_proxy = "http://corporate.proxy:3128"

  ## HTTP User-Agent
  # user_agent = "telegraf"

  ## Content-Encoding for write request body, can be set to "gzip" to
  ## compress body or "identity" to apply no encoding.
  # content_encoding = "gzip"

  ## Enable or disable uint support for writing uints influxdb 2.0.
  # influx_uint_support = false

  ## Optional TLS Config for use on HTTP connections.
  # tls_ca = "/etc/telegraf/ca.pem"
  # tls_cert = "/etc/telegraf/cert.pem"
  # tls_key = "/etc/telegraf/key.pem"
  ## Use TLS but skip chain & host verification
  # insecure_skip_verify = false


[[inputs.gnmi]]
  ## Address and port of the GNMI GRPC server
  addresses = ["x.x.x.x:6030"]

  ## credentials
  username = "c3edge"
  password = "Welcome@888!"

  ## redial in case of failures after
  redial = "10s"

  [[inputs.gnmi.subscription]]
    ## Name of the measurement
    name = "ifcounters"

    origin = "openconfig"
    path = "/interfaces/interface/state/counters"

    subscription_mode = "sample"
    sample_interval = "16s"
--------------------
----------------------------    //telegraf set up ---test arista gnmi
 [[outputs.influxdb_v2]]
  ## The URLs of the InfluxDB cluster nodes.
  ##
  ## Multiple URLs can be specified for a single cluster, only ONE of the
  ## urls will be written to each interval.
  ##   ex: urls = ["https://us-west-2-1.aws.cloud2.influxdata.com"]
  urls = ["http://x.x.x.x:8086"]

  ## API token for authentication.
  token = "0MyZWaVdTYe_xb-tC1uVBniogW_echPzDrUC2vTn9PTHfqXci-NgPgJhb0x4zVJTTviBjxduAwsMxZKx172-mQ=="

  ## Organization is the name of the organization you wish to write to; must exist.
  organization = "influxdb"

  ## Destination bucket to write into.
  bucket = "jti"

  ## The value of this tag will be used to determine the bucket.  If this
  ## tag is not set the 'bucket' option is used as the default.
  # bucket_tag = ""

  ## If true, the bucket tag will not be added to the metric.
  # exclude_bucket_tag = false

  ## Timeout for HTTP messages.
  # timeout = "5s"

  ## Additional HTTP headers
  # http_headers = {"X-Special-Header" = "Special-Value"}

  ## HTTP Proxy override, if unset values the standard proxy environment
  ## variables are consulted to determine which proxy, if any, should be used.
  # http_proxy = "http://corporate.proxy:3128"

  ## HTTP User-Agent
  # user_agent = "telegraf"

  ## Content-Encoding for write request body, can be set to "gzip" to
  ## compress body or "identity" to apply no encoding.
  # content_encoding = "gzip"

  ## Enable or disable uint support for writing uints influxdb 2.0.
  # influx_uint_support = false

  ## Optional TLS Config for use on HTTP connections.
  # tls_ca = "/etc/telegraf/ca.pem"
  # tls_cert = "/etc/telegraf/cert.pem"
  # tls_key = "/etc/telegraf/key.pem"
  ## Use TLS but skip chain & host verification
  # insecure_skip_verify = false


[[inputs.jti_openconfig_telemetry]]
  ## List of device addresses to collect telemetry from
  servers = ["x.x.x.x:50051"]

  ## Authentication details. Username and password are must if device expects
  ## authentication. Client ID must be unique when connecting from multiple instances
  ## of telegraf to the same device
  username = "c3edge"
  password = "Welcome@888!"
  client_id = "telegraf"

  ## Frequency to get data
  sample_frequency = "10000ms"

  ## Sensors to subscribe for
  ## A identifier for each sensor can be provided in path by separating with space
  ## Else sensor path will be used as identifier
  ## When identifier is used, we can provide a list of space separated sensors.
  ## A single subscription will be created with all these sensors and data will
  ## be saved to measurement with this identifier name
  sensors = [
   "/interfaces/"
  ]

  ## We allow specifying sensor group level reporting rate. To do this, specify the
  ## reporting rate in Duration at the beginning of sensor paths / collection
  ## name. For entries without reporting rate, we use configured sample frequency
  #sensors = [
  # "1000ms customReporting /interfaces /lldp",
  # "2000ms collection /components",
  # "/interfaces",
  #]

  ## Optional TLS Config
  # enable_tls = true
  # tls_ca = "/etc/telegraf/ca.pem"
  # tls_cert = "/etc/telegraf/cert.pem"
  # tls_key = "/etc/telegraf/key.pem"
  ## Use TLS but skip chain & host verification
  # insecure_skip_verify = false

  ## Delay between retry attempts of failed RPC calls or streams. Defaults to 1000ms.
  ## Failed streams/calls will not be retried if 0 is provided
  #retry_delay = "1000ms"

  ## To treat all string values as tags, set this to true
  str_as_tags = false

Attention:
1. Juniper device need configure netconf/extensible service/allow tcp port 

set system services netconf ssh
set system services netconf rfc-compliant
set system services netconf yang-compliant
set system services extension-service request-response grpc clear-text port 50051
set system services extension-service request-response grpc skip-authentication
set system services extension-service notification allow-clients address 0.0.0.0/0

2. confirm tcp connection
show system connections |match 50051

3. confirm sensor
show ephemeral-configuration instance junos-analytics

show ephemeral-configuration
show agent sensors | no-more

show services rpm twamp client connection   //not for basic verification of juniper telemetry
show services rpm twamp client probe-results  //not for basic verification of juniper telemetry

4. telegraf tip:
use the self made steps to install telegraf, during which includes the source code of third partes can enable telegraf supportting new plugins

5. influxdb
web UI can show you the way of configuring telegraf for any plugins it support, can select query satement , if you need extract tagname/tagvalue/labelname/labelvaue
use for grafana, can use the function "schema" which can be referrenced in influxdb guidelines

6. grafana
grafana support many tempelates, can import them use ID/Name, using these template dashboards/panels can learn how to customise your own dashboards/panel

use variable feature and repeat in panel editting page can iterate all the series date you filtered from influxdb

grafana support expressions and transformation, by which can restructure the date to fit the representation you want to see

=====================================
will update again when I learn something new ......



