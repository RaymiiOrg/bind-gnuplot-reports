# Bind log Graphs
## with GNUplot

<img src="https://raymii.org/s/inc/img/dns-report.png" />

This is a very simple script which uses GNUplot to create graphs of Bind Query logs.

## Bind logging
Enable query logging in Bind:

    # /etc/bind/named.conf.options
         logging{
           channel system_log {
             file "/var/log/named/sys.log" versions unlimited size 2g;
             severity warning;
             print-time yes;
             print-severity yes;
             print-category yes;
           };
           channel queries_log {
             file "/var/log/named/queries.log" versions unlimited size 2g;
             severity info;
             print-time no;
           };
           category default{
             system_log;
           };
           category queries{
             queries_log;
           };
         };

## Bind log parsing

Use the following command line to get the 20 most queries domains  

    awk '{ print $4 }' /var/log/named/queries.log | sort | uniq -c | sort -n | tail -n 20 > dns-data

Example data:

    83094 metrics-api.librato.com
    83689 collector-2.newrelic.com
    84165 puppetmaster.int
    82445 ntp0.nl.net

The use the gnuplot script to create the graph:

    gnuplot < plot.gplt

And there you go.

Because GNUplot has no easy support for horizotal bar graphs, if you want it horizontal, rotate it with Imagemagick:

    convert data.png -rotate 90 data-90.png

And thats it, you now have a nice image overview of DNS queries.

## Links

- Raymii.org: https://raymii.org/s/software/Bind-GNUPlot-DNS-Bar-Graph.html
- Github: https://github.com/RaymiiOrg/bind-gnuplot-reports
