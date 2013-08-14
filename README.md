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

Remember to create the `/var/log/named/` folder (where in Ubuntu 12.04 Apparmor allows the bind user to write by default):

    mkdir /var/log/named
    chown bind:bind /var/log/named

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

- [Raymii.org](https://raymii.org/s/software/Bind-GNUPlot-DNS-Bar-Graph.html)
- [Github](https://github.com/RaymiiOrg/bind-gnuplot-reports)

## License

    Copyright (C) Remy van Elst 2013

    This program is free software: you can redistribute it and/or modify
    it under the terms of the GNU General Public License as published by
    the Free Software Foundation, either version 3 of the License, or
    (at your option) any later version.

    This program is distributed in the hope that it will be useful,
    but WITHOUT ANY WARRANTY; without even the implied warranty of
    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    GNU General Public License for more details.

    You should have received a copy of the GNU General Public License
    along with this program.  If not, see <http://www.gnu.org/licenses/>.
