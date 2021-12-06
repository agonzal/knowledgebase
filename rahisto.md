I added 95th percentile reporting to rahisto(), which is our frequency  
distribution
tool.  This tool calculates mean, stddev, max, min, and the median (50th
percentile), and so it was very easy to add 95th percentile to the  
report.

You feed rahisto() the output of your 5 minute rabins() aggregations,
and it will give you a little stats report of the specific variable, and
a frequency distribution of where the data falls.  If you don't know  
what
the range is, run it with just a small number of bins, and rahisto()  
will start
to show you where the data lies in its range.

Using the examples I used before:

        rabins -M rmon hard time 5m -m smac -r hourly.file -w /tmp/ data.out

Now, run rahisto() instead of rasort(), this way to generate your  
OutBound data
for the specific ether address ('sload') :

    rahisto -r /tmp/data.out -H sload 10 - ether src host  0:a0:c5:e1:7a:fa

`  N = 31      mean =  80407.974516  stddev =  67795.873860  max =  
174059.203125  min = 172.133331
            median =  82742.617188     95% = 173895.046875
  Class           Interval                Freq    Rel.Freq     Cum.Freq
      1   0.000000e+00-1.740600e+04         12    38.7097%     38.7097%
      2   1.740600e+04-3.481200e+04          0     0.0000%     38.7097%
      3   3.481200e+04-5.221800e+04          3     9.6774%     48.3871%
      4   5.221800e+04-6.962400e+04          1     3.2258%     51.6129%
      5   6.962400e+04-8.703000e+04          1     3.2258%     54.8387%
      6   8.703000e+04-1.044360e+05          0     0.0000%     54.8387%
      7   1.044360e+05-1.218420e+05          1     3.2258%     58.0645%
      8   1.218420e+05-1.392480e+05          3     9.6774%     67.7419%
      9   1.392480e+05-1.566540e+05          4    12.9032%     80.6452%
     10   1.566540e+05-1.740600e+05          6    19.3548%    100.0000%
`
And, run rahisto() this way to generate your InBound data for the  
specific ether address ('dload') :

    rahisto -r /tmp/data.out -H dload 10 - ether src host  0:a0:c5:e1:7a:fa
`  N = 31      mean = 2520065.831098  stddev = 2286667.779977  max =  
5742157.000000  min = 335.946655
            median = 2935971.500000     95% = 5711441.500000
  Class           Interval                Freq    Rel.Freq     Cum.Freq
      1   0.000000e+00-5.742160e+05         13    41.9355%     41.9355%
      2   5.742160e+05-1.148432e+06          1     3.2258%     45.1613%
      3   1.148432e+06-1.722648e+06          1     3.2258%     48.3871%
      4   1.722648e+06-2.296864e+06          1     3.2258%     51.6129%
      5   2.296864e+06-2.871080e+06          0     0.0000%     51.6129%
      6   2.871080e+06-3.445296e+06          2     6.4516%     58.0645%
      7   3.445296e+06-4.019512e+06          1     3.2258%     61.2903%
      8   4.019512e+06-4.593728e+06          3     9.6774%     70.9677%
      9   4.593728e+06-5.167944e+06          3     9.6774%     80.6452%
     10   5.167944e+06-5.742160e+06          6    19.3548%    100.0000%`


The numbers are slightly different from the last time, because of the  
bug in rabins().

So you see, the data I'm using is multi-modally distributed, and while  
very low in samples,
it does suggest  an SLA for two tiers, one above 1M bps and one below  
1M bps.
You can calculate a 95th percentile for the two regions, by adjusting  
the range on the
histogram option field like this (just do dload for this example):

Traffic Below 1M bps
 ` rahisto -r /tmp/rabins.5m.out -H dload 10:0-1M - ip and ether src  host 0:a0:c5:e1:7a:fa`
 ` N = 14      mean = 220514.849217  stddev = 168862.596105  max =  
612549.625000  min = 335.946655
            median = 157696.734375     95% = 612549.625000
  Class           Interval                Freq    Rel.Freq     Cum.Freq
      1   0.000000e+00-1.000000e+05          2    14.2857%     14.2857%
      2   1.000000e+05-2.000000e+05          7    50.0000%     64.2857%
      3   2.000000e+05-3.000000e+05          1     7.1429%     71.4286%
      4   3.000000e+05-4.000000e+05          2    14.2857%     85.7143%
      5   4.000000e+05-5.000000e+05          1     7.1429%     92.8571%
      6   5.000000e+05-6.000000e+05          0     0.0000%     92.8571%
      7   6.000000e+05-7.000000e+05          1     7.1429%    100.0000%
      8   7.000000e+05-8.000000e+05          0     0.0000%    100.0000%
      9   8.000000e+05-9.000000e+05          0     0.0000%    100.0000%
     10   9.000000e+05-1.000000e+06          0     0.0000%    100.0000%`

Traffic Above 1M bps
`  rahisto -r /tmp/rabins.5m.out -H dload 10:1-6M - ip and ether src  host 0:a0:c5:e1:7a:fa`
`  N = 17      mean = 4413813.698529  stddev = 1253167.017151  max =  
5742157.000000  min = 1717778.125000
            median = 4992350.500000     95% = 5742157.000000
  Class           Interval                Freq    Rel.Freq     Cum.Freq
      1   1.000000e+06-1.500000e+06          0     0.0000%      0.0000%
      2   1.500000e+06-2.000000e+06          2    11.7647%     11.7647%
      3   2.000000e+06-2.500000e+06          0     0.0000%     11.7647%
      4   2.500000e+06-3.000000e+06          1     5.8824%     17.6471%
      5   3.000000e+06-3.500000e+06          1     5.8824%     23.5294%
      6   3.500000e+06-4.000000e+06          1     5.8824%     29.4118%
      7   4.000000e+06-4.500000e+06          1     5.8824%     35.2941%
      8   4.500000e+06-5.000000e+06          4    23.5294%     58.8235%
      9   5.000000e+06-5.500000e+06          3    17.6471%     76.4706%
     10   5.500000e+06-6.000000e+06          4    23.5294%    100.0000%`