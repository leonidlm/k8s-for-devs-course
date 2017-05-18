# Useful commands & links

# Instructions

When running applications in production K8s environment it's important to make
sure the system will properly restart and proxy traffic to your containers.

Let's add liveness and readiness probes for both wordpress and mysql containers
from the previous exercises.

The wordress container can be considered alive when it continues to successfully
responds to http requests.

At the moment we won't add a relevant readiness probe to the configuration since
we aren't concerned about it's boot up time.

Mysql container can be considered alive as long as it successfully responds to
tcp connection on it's default (3306) port.

Sometimes it may take a few seconds for the mysql container to start serving
requests. Add the relevant readiness probe to make sure that it won't receive
requests from the Wordpress containers in the meantime.

