# IPV6

### Basics

Address are split into: 

* Unicast 
  * Global - similar to IPv4 public IP addresses. They have a prefix of 2000::/3 
  * Unique local - similar to IPv4 private addresses. There addresses have a prefix of FD00::/8 or FC00::/7 
  * Link  local - there addresses are used for sending packet over the local subnet. There addresses have a prefix of FE80::/10. 
* Anycast 
* Multicast  - prefix with ff00::/8 

Packets sent to ::1 are sent to localhost 

Packets sent to ::0 are sent on all interfaces. 

Designated multicast address \(just a few for example\): 

| Address  | Scope  | Use  |
| :--- | :--- | :--- |
| ff02::1  | Link  | All nodes  |
| ff02::2  | Link  | All routers  |
| ff02::5  | Link  | OSPF routers  |
| ff02::a  | Link  | EIGRP routers  |

From &lt;[https://github.com/0xbharath/talks/blob/master/pentesting\_ipv6/pentesting\_IPv6.md](https://github.com/0xbharath/talks/blob/master/pentesting_ipv6/pentesting_IPv6.md)&gt;  

### IPv6 Address Types

{% file src="../../../.gitbook/assets/ipv6\_reference\_card.pdf" %}

### IPv6 Command Line Tools: 

* ping6- IPv6 ping tool 
* traceroute6- IPv6 tracing tool 
* tracepath6 – IPv6 tracing tool 
* ip -6 – For configuring/viewing IPv6 interfaces and routes 
* ipv6calc – IPv6 subnet calculator 
* tcpdump ip6 – packet sniffing on IPv6 
* snoop inet6 – packet sniffing on IPv6 

### IPv6 Subnet Size Reference Table

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>IPv6 CIDR S</p>
        <p>Subnet</p>
      </th>
      <th style="text-align:left">Number of IPs</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">/128</td>
      <td style="text-align:left">1</td>
    </tr>
    <tr>
      <td style="text-align:left">/127</td>
      <td style="text-align:left">2</td>
    </tr>
    <tr>
      <td style="text-align:left">/126</td>
      <td style="text-align:left">4</td>
    </tr>
    <tr>
      <td style="text-align:left">/125</td>
      <td style="text-align:left">8</td>
    </tr>
    <tr>
      <td style="text-align:left">/124</td>
      <td style="text-align:left">16</td>
    </tr>
    <tr>
      <td style="text-align:left">/123</td>
      <td style="text-align:left">32</td>
    </tr>
    <tr>
      <td style="text-align:left">/122</td>
      <td style="text-align:left">64</td>
    </tr>
    <tr>
      <td style="text-align:left">/121</td>
      <td style="text-align:left">128</td>
    </tr>
    <tr>
      <td style="text-align:left">/120</td>
      <td style="text-align:left">256</td>
    </tr>
    <tr>
      <td style="text-align:left">/119</td>
      <td style="text-align:left">512</td>
    </tr>
    <tr>
      <td style="text-align:left">/118</td>
      <td style="text-align:left">1,024</td>
    </tr>
    <tr>
      <td style="text-align:left">/117</td>
      <td style="text-align:left">2,048</td>
    </tr>
    <tr>
      <td style="text-align:left">/116</td>
      <td style="text-align:left">4,096</td>
    </tr>
    <tr>
      <td style="text-align:left">/115</td>
      <td style="text-align:left">8,192</td>
    </tr>
    <tr>
      <td style="text-align:left">/114</td>
      <td style="text-align:left">16,384</td>
    </tr>
    <tr>
      <td style="text-align:left">/113</td>
      <td style="text-align:left">32,768</td>
    </tr>
    <tr>
      <td style="text-align:left">/112</td>
      <td style="text-align:left">65,536</td>
    </tr>
    <tr>
      <td style="text-align:left">/111</td>
      <td style="text-align:left">131,072</td>
    </tr>
    <tr>
      <td style="text-align:left">/110</td>
      <td style="text-align:left">262,144</td>
    </tr>
    <tr>
      <td style="text-align:left">/109</td>
      <td style="text-align:left">524,288</td>
    </tr>
    <tr>
      <td style="text-align:left">/108</td>
      <td style="text-align:left">1,048,576</td>
    </tr>
    <tr>
      <td style="text-align:left">/107</td>
      <td style="text-align:left">2,097,152</td>
    </tr>
    <tr>
      <td style="text-align:left">/106</td>
      <td style="text-align:left">4,194,304</td>
    </tr>
    <tr>
      <td style="text-align:left">/105</td>
      <td style="text-align:left">8,388,608</td>
    </tr>
    <tr>
      <td style="text-align:left">/104</td>
      <td style="text-align:left">16,777,216</td>
    </tr>
    <tr>
      <td style="text-align:left">/103</td>
      <td style="text-align:left">33,554,432</td>
    </tr>
    <tr>
      <td style="text-align:left">/102</td>
      <td style="text-align:left">67,108,864</td>
    </tr>
    <tr>
      <td style="text-align:left">/101</td>
      <td style="text-align:left">134,217,728</td>
    </tr>
    <tr>
      <td style="text-align:left">/100</td>
      <td style="text-align:left">268,435,456</td>
    </tr>
    <tr>
      <td style="text-align:left">/99</td>
      <td style="text-align:left">536,870,912</td>
    </tr>
    <tr>
      <td style="text-align:left">/98</td>
      <td style="text-align:left">1,073,741,824</td>
    </tr>
    <tr>
      <td style="text-align:left">/97</td>
      <td style="text-align:left">2,147,483,648</td>
    </tr>
    <tr>
      <td style="text-align:left">/96</td>
      <td style="text-align:left">4,294,967,296</td>
    </tr>
    <tr>
      <td style="text-align:left">/95</td>
      <td style="text-align:left">8,589,934,592</td>
    </tr>
    <tr>
      <td style="text-align:left">/94</td>
      <td style="text-align:left">17,179,869,184</td>
    </tr>
    <tr>
      <td style="text-align:left">/93</td>
      <td style="text-align:left">34,359,738,368</td>
    </tr>
    <tr>
      <td style="text-align:left">/92</td>
      <td style="text-align:left">68,719,476,736</td>
    </tr>
    <tr>
      <td style="text-align:left">/91</td>
      <td style="text-align:left">137,438,953,472</td>
    </tr>
    <tr>
      <td style="text-align:left">/90</td>
      <td style="text-align:left">274,877,906,944</td>
    </tr>
    <tr>
      <td style="text-align:left">/89</td>
      <td style="text-align:left">549,755,813,888</td>
    </tr>
    <tr>
      <td style="text-align:left">/88</td>
      <td style="text-align:left">1,099,511,627,776</td>
    </tr>
    <tr>
      <td style="text-align:left">/87</td>
      <td style="text-align:left">2,199,023,255,552</td>
    </tr>
    <tr>
      <td style="text-align:left">/86</td>
      <td style="text-align:left">4,398,046,511,104</td>
    </tr>
    <tr>
      <td style="text-align:left">/85</td>
      <td style="text-align:left">8,796,093,022,208</td>
    </tr>
    <tr>
      <td style="text-align:left">/84</td>
      <td style="text-align:left">17,592,186,044,416</td>
    </tr>
    <tr>
      <td style="text-align:left">/83</td>
      <td style="text-align:left">35,184,372,088,832</td>
    </tr>
    <tr>
      <td style="text-align:left">/82</td>
      <td style="text-align:left">70,368,744,177,664</td>
    </tr>
    <tr>
      <td style="text-align:left">/81</td>
      <td style="text-align:left">140,737,488,355,328</td>
    </tr>
    <tr>
      <td style="text-align:left">/80</td>
      <td style="text-align:left">281,474,976,710,656</td>
    </tr>
    <tr>
      <td style="text-align:left">/79</td>
      <td style="text-align:left">562,949,953,421,312</td>
    </tr>
    <tr>
      <td style="text-align:left">/78</td>
      <td style="text-align:left">1,125,899,906,842,624</td>
    </tr>
    <tr>
      <td style="text-align:left">/77</td>
      <td style="text-align:left">2,251,799,813,685,248</td>
    </tr>
    <tr>
      <td style="text-align:left">/76</td>
      <td style="text-align:left">4,503,599,627,370,496</td>
    </tr>
    <tr>
      <td style="text-align:left">/75</td>
      <td style="text-align:left">9,007,199,254,740,992</td>
    </tr>
    <tr>
      <td style="text-align:left">/74</td>
      <td style="text-align:left">18,014,398,509,481,985</td>
    </tr>
    <tr>
      <td style="text-align:left">/73</td>
      <td style="text-align:left">36,028,797,018,963,968</td>
    </tr>
    <tr>
      <td style="text-align:left">/72</td>
      <td style="text-align:left">72,057,594,037,927,936</td>
    </tr>
    <tr>
      <td style="text-align:left">/71</td>
      <td style="text-align:left">144,115,188,075,855,872</td>
    </tr>
    <tr>
      <td style="text-align:left">/70</td>
      <td style="text-align:left">288,230,376,151,711,744</td>
    </tr>
    <tr>
      <td style="text-align:left">/69</td>
      <td style="text-align:left">576,460,752,303,423,488</td>
    </tr>
    <tr>
      <td style="text-align:left">/68</td>
      <td style="text-align:left">1,152,921,504,606,846,976</td>
    </tr>
    <tr>
      <td style="text-align:left">/67</td>
      <td style="text-align:left">2,305,843,009,213,693,952</td>
    </tr>
    <tr>
      <td style="text-align:left">/66</td>
      <td style="text-align:left">4,611,686,018,427,387,904</td>
    </tr>
    <tr>
      <td style="text-align:left">/65</td>
      <td style="text-align:left">9,223,372,036,854,775,808</td>
    </tr>
    <tr>
      <td style="text-align:left">Residential &#x2013; /64</td>
      <td style="text-align:left">18,446,744,073,709,551,616</td>
    </tr>
    <tr>
      <td style="text-align:left">/63</td>
      <td style="text-align:left">36,893,488,147,419,103,232</td>
    </tr>
    <tr>
      <td style="text-align:left">/62</td>
      <td style="text-align:left">73,786,976,294,838,206,464</td>
    </tr>
    <tr>
      <td style="text-align:left">/61</td>
      <td style="text-align:left">147,573,952,589,676,412,928</td>
    </tr>
    <tr>
      <td style="text-align:left">/60</td>
      <td style="text-align:left">295,147,905,179,352,825,856</td>
    </tr>
    <tr>
      <td style="text-align:left">/59</td>
      <td style="text-align:left">590,295,810,358,705,651,712</td>
    </tr>
    <tr>
      <td style="text-align:left">/58</td>
      <td style="text-align:left">1,180,591,620,717,411,303,424</td>
    </tr>
    <tr>
      <td style="text-align:left">/57</td>
      <td style="text-align:left">2,361,183,241,434,822,606,848</td>
    </tr>
    <tr>
      <td style="text-align:left">/56</td>
      <td style="text-align:left">4,722,366,482,869,645,213,696</td>
    </tr>
    <tr>
      <td style="text-align:left">/55</td>
      <td style="text-align:left">9,444,732,965,739,290,427,392</td>
    </tr>
    <tr>
      <td style="text-align:left">/54</td>
      <td style="text-align:left">18,889,465,931,478,580,854,784</td>
    </tr>
    <tr>
      <td style="text-align:left">/53</td>
      <td style="text-align:left">37,778,931,862,957,161,709,568</td>
    </tr>
    <tr>
      <td style="text-align:left">/52</td>
      <td style="text-align:left">75,557,863,725,914,323,419,136</td>
    </tr>
    <tr>
      <td style="text-align:left">/51</td>
      <td style="text-align:left">151,115,727,451,828,646,838,272</td>
    </tr>
    <tr>
      <td style="text-align:left">/50</td>
      <td style="text-align:left">302,231,454,903,657,293,676,544</td>
    </tr>
    <tr>
      <td style="text-align:left">/49</td>
      <td style="text-align:left">604,462,909,807,314,587,353,088</td>
    </tr>
    <tr>
      <td style="text-align:left">Business &#x2013; /48</td>
      <td style="text-align:left">1,208,925,819,614,629,174,706,176</td>
    </tr>
    <tr>
      <td style="text-align:left">/47</td>
      <td style="text-align:left">2,417,851,639,229,258,349,412,352</td>
    </tr>
    <tr>
      <td style="text-align:left">/46</td>
      <td style="text-align:left">4,835,703,278,458,516,698,824,704</td>
    </tr>
    <tr>
      <td style="text-align:left">/45</td>
      <td style="text-align:left">9,671,406,556,917,033,397,649,408</td>
    </tr>
    <tr>
      <td style="text-align:left">/44</td>
      <td style="text-align:left">19,342,813,113,834,066,795,298,816</td>
    </tr>
    <tr>
      <td style="text-align:left">/43</td>
      <td style="text-align:left">38,685,626,227,668,133,590,597,632</td>
    </tr>
    <tr>
      <td style="text-align:left">/42</td>
      <td style="text-align:left">77,371,252,455,336,267,181,195,264</td>
    </tr>
    <tr>
      <td style="text-align:left">/41</td>
      <td style="text-align:left">154,742,504,910,672,534,362,390,528</td>
    </tr>
    <tr>
      <td style="text-align:left">/40</td>
      <td style="text-align:left">309,485,009,821,345,068,724,781,056</td>
    </tr>
    <tr>
      <td style="text-align:left">/39</td>
      <td style="text-align:left">618,970,019,642,690,137,449,562,112</td>
    </tr>
    <tr>
      <td style="text-align:left">/38</td>
      <td style="text-align:left">1,237,940,039,285,380,274,899,124,224</td>
    </tr>
    <tr>
      <td style="text-align:left">/37</td>
      <td style="text-align:left">2,475,880,078,570,760,549,798,248,448</td>
    </tr>
    <tr>
      <td style="text-align:left">/36</td>
      <td style="text-align:left">4,951,760,157,141,521,099,596,496,896</td>
    </tr>
    <tr>
      <td style="text-align:left">/35</td>
      <td style="text-align:left">9,903,520,314,283,042,199,192,993,792</td>
    </tr>
    <tr>
      <td style="text-align:left">/34</td>
      <td style="text-align:left">19,807,040,628,566,084,398,385,987,584</td>
    </tr>
    <tr>
      <td style="text-align:left">/33</td>
      <td style="text-align:left">39,614,081,257,132,168,796,771,975,168</td>
    </tr>
    <tr>
      <td style="text-align:left">ISP &#x2013; /32</td>
      <td style="text-align:left">79,228,162,514,264,337,593,543,950,336</td>
    </tr>
    <tr>
      <td style="text-align:left">/31</td>
      <td style="text-align:left">158,456,325,028,528,675,187,087,900,672</td>
    </tr>
    <tr>
      <td style="text-align:left">/30</td>
      <td style="text-align:left">316,912,650,057,057,350,374,175,801,344</td>
    </tr>
    <tr>
      <td style="text-align:left">/29</td>
      <td style="text-align:left">633,825,300,114,114,700,748,351,602,688</td>
    </tr>
    <tr>
      <td style="text-align:left">/28</td>
      <td style="text-align:left">1,267,650,600,228,229,401,496,703,205,376</td>
    </tr>
    <tr>
      <td style="text-align:left">/27</td>
      <td style="text-align:left">2,535,301,200,456,458,802,993,406,410,752</td>
    </tr>
    <tr>
      <td style="text-align:left">/26</td>
      <td style="text-align:left">5,070,602,400,912,917,605,986,812,821,504</td>
    </tr>
    <tr>
      <td style="text-align:left">/25</td>
      <td style="text-align:left">10,141,204,801,825,835,211,973,625,643,008</td>
    </tr>
    <tr>
      <td style="text-align:left">/24</td>
      <td style="text-align:left">20,282,409,603,651,670,423,947,251,286,016</td>
    </tr>
    <tr>
      <td style="text-align:left">/23</td>
      <td style="text-align:left">40,564,819,207,303,340,847,894,502,572,032</td>
    </tr>
    <tr>
      <td style="text-align:left">/22</td>
      <td style="text-align:left">81,129,638,414,606,681,695,789,005,144,064</td>
    </tr>
    <tr>
      <td style="text-align:left">/21</td>
      <td style="text-align:left">162,259,276,829,213,363,391,578,010,288,128</td>
    </tr>
    <tr>
      <td style="text-align:left">/20</td>
      <td style="text-align:left">324,518,553,658,426,726,783,156,020,576,256</td>
    </tr>
    <tr>
      <td style="text-align:left">/19</td>
      <td style="text-align:left">649,037,107,316,853,453,566,312,041,152,512</td>
    </tr>
    <tr>
      <td style="text-align:left">/18</td>
      <td style="text-align:left">1,298,074,214,633,706,907,132,624,082,305,024</td>
    </tr>
    <tr>
      <td style="text-align:left">/17</td>
      <td style="text-align:left">2,596,148,429,267,413,814,265,248,164,610,048</td>
    </tr>
    <tr>
      <td style="text-align:left">/16</td>
      <td style="text-align:left">5,192,296,858,534,827,628,530,496,329,220,096</td>
    </tr>
    <tr>
      <td style="text-align:left">/15</td>
      <td style="text-align:left">10,384,593,717,069,655,257,060,992,658,440,192</td>
    </tr>
    <tr>
      <td style="text-align:left">/14</td>
      <td style="text-align:left">20,769,187,434,139,310,514,121,985,316,880,384</td>
    </tr>
    <tr>
      <td style="text-align:left">/13</td>
      <td style="text-align:left">41,538,374,868,278,621,028,243,970,633,760,768</td>
    </tr>
    <tr>
      <td style="text-align:left">/12</td>
      <td style="text-align:left">83,076,749,736,557,242,056,487,941,267,521,536</td>
    </tr>
    <tr>
      <td style="text-align:left">/11</td>
      <td style="text-align:left">166,153,499,473,114,484,112,975,882,535,043,072</td>
    </tr>
    <tr>
      <td style="text-align:left">/10</td>
      <td style="text-align:left">332,306,998,946,228,968,225,951,765,070,086,144</td>
    </tr>
    <tr>
      <td style="text-align:left">/9</td>
      <td style="text-align:left">664,613,997,892,457,936,451,903,530,140,172,288</td>
    </tr>
    <tr>
      <td style="text-align:left">/8</td>
      <td style="text-align:left">1,329,227,995,784,915,872,903,807,060,280,344,576</td>
    </tr>
  </tbody>
</table>### IPv6 Subnet Reference Prefix Lengths

```text
2402:9400:0000:0000:0000:0000:0000:0001 
XXXX:XXXX:XXXX:XXXX:XXXX:XXXX:XXXX:XXXX 
      ||| |||| |||| |||| |||| |||| |||| 
      ||| |||| |||| |||| |||| |||| |||128 
      ||| |||| |||| |||| |||| |||| ||124 
      ||| |||| |||| |||| |||| |||| |120 
      ||| |||| |||| |||| |||| |||| 116 
      ||| |||| |||| |||| |||| |||112 
      ||| |||| |||| |||| |||| ||108 
      ||| |||| |||| |||| |||| |104 
      ||| |||| |||| |||| |||| 100 
      ||| |||| |||| |||| |||96 
      ||| |||| |||| |||| ||92 
      ||| |||| |||| |||| |88 
      ||| |||| |||| |||| 84 
      ||| |||| |||| |||80 
      ||| |||| |||| ||76 
      ||| |||| |||| |72 
      ||| |||| |||| 68 
      ||| |||| |||64 
      ||| |||| ||60 
      ||| |||| |56 
      ||| |||| 52 
      ||| |||48 
      ||| ||44 
      ||| |40 
      ||| 36 
      ||32 
      |28 
      24 
```

Note: The IP address above is an IP address allocated to Crucial Paradigm. 

### Example of /64 Allocations: 

/64 IPv6 allocations are usually given to end users, who do not require any VLANs.  It allows auto configuration, or SLAAC so makes life a lot easier when configuring. 

It is fairly easy to calculate /64 allocations, and a subnet calculator is not required. In fact this is the case with assigning IPv6 allocations, it can be done fairly easily without any calculator \(I’ll demonstrate this later in the reference sheet\): 

```text
2402:9400:1000:0::/64 
2402:9400:1000:1::/64 
2402:9400:1000:2::/64 
2402:9400:1000:3::/64 
2402:9400:1000:4::/64 
2402:9400:1000:5::/64 
2402:9400:1000:6::/64 
2402:9400:1000:7::/64 
2402:9400:1000:8::/64 
2402:9400:1000:9::/64 
2402:9400:1000:a::/64 
2402:9400:1000:b::/64 
2402:9400:1000:c::/64 
2402:9400:1000:e::/64 
2402:9400:1000:e::/64 
2402:9400:1000:f::/64 
2402:9400:1000:10::/64 
2402:9400:1000:11::/64 
```

Example of /48 Allocations: 

/48 allocations are usually provided to business, who require additional VLANs or may require the range to be split up.  Using a /48 allocation would allow them to do so. 

```text
2402:9400:10::/48 
2402:9400:11::/48 
2402:9400:12::/48 
2402:9400:13::/48 
2402:9400:14::/48 
2402:9400:15::/48 
2402:9400:16::/48 
2402:9400:17::/48 
2402:9400:18::/48 
2402:9400:19::/48 
2402:9400:1a::/48 
2402:9400:1b::/48 
2402:9400:1c::/48 
2402:9400:1e::/48 
2402:9400:1f::/48 
2402:9400:20::/48 
```

### IPv6 Subnet Calculator NOT REQUIRED! 

In most cases a subnet calculator will not be required, since IPv6 using hex \(hexadecimal\) – and so long as the prefix length is a multiple of 4, it makes it quite easy.  For example \(this is also where the table “IPv6 Subnet Reference IP Address” comes in a lot of handy above\): 

```text
2402:9400:1234:1234::/64 
2402:9400:1234:123X::/60 
2402:9400:1234:12XX::/56 
2402:9400:1234:1XXX::/52 
2402:9400:1234:XXXX::/48 
2402:9400:123X:XXXX::/44 
2402:9400:12XX:XXXX::/40 
```

### IPv6 Address Scopes

::/128 unspecified address 

::1/128 localhost 

fe80::/10 link local 

fc00::/7 unique local unicast  \(RFC 4193\) 

fc00::/8 centrally assigned by unkown, routed within a site \(RFC 4193\) 

fd00::/8 free for all, global ID must be generated randomly with pseudo-random algorithm, routed within a site \(RFC 4193\) 

ff00::/8 multicast, following after the prefix ff there are 4 bits for flags and 4 bits for the scope 

::ffff:0:0/96 IPv4 to IPv6 Address, eg: ::ffff:10.10.10.10 \(RFC 4038\) 

2000::/3 global unicast 

2001::/16 /32 subnets assigned to providers, they assign /48, /56 or /64 to the customer 

2001:db8::/32 reserved for use in documentation 

2001:678::/29 Provider Independent \(PI\) adresses and anycasting TLD nameservers 

2002::/16 6to4 scope, 2002:c058:6301:: is the 6to4 public router anycast \(RFC 3068\) 

### Interface Configuration Linux: 

`#ifconfig eth0 inet6 add 2402:9400:1234:1234::1/64` 

Configuring SLAAC \(auto configuration\) on Redhat/CentOS flavours of Linux: You can do this by enabling IPv6 on an interface which is already configured automatically on boot. 

