# <h1>EIGRP : eigrp authentication; ip summarisation; Injecting Default Route</h1>

<h2>Description</h2>
Project consists of onjectives such as eigrp authentication; ip summarisation; Injecting Default Route all through eigrp
<br />
<h2>Lab:</h2>
<p align="center">
PROJECT:<br/>
![Lab Setup](lab.png)

<h2>🔐 1. Injecting Default route</h2>
<div class="code">
<div style="font-size: 14px; line-height: 1.4; max-width: 600px;">
<p><strong>EIGRP Default Routes</strong><br>
EIGRP injects defaults via <em>static redistribution</em> or <em>IP summarization</em> (unlike RIP's simple <code>default-information originate</code>).</p>

<h4>Static Redistribution</h4>
<p>Add <code>ip route 0.0.0.0 0.0.0.0 Null0</code><br>
Then <code>redistribute static</code> in EIGRP.<br>
→ Propagates <code>0.0.0.0/0</code> as <strong>external E2 (AD 170)</strong>.</p>

<h4>IP Summarization</h4>
<p><code>ip summary-address eigrp &lt;AS&gt; 0.0.0.0 0.0.0.0</code> on outbound interface.<br>
→ Advertises default <em>only to neighbors</em>.<br>
→ Creates local <code>Null0</code> discard route.</p>
</div>



<h2>🔐 2. EIGRP Authentication</h2>
<div class="code">
key chain EIGRP-SEC<br>
 key 1<br>
  key-string CCNA2026!<br>
int g0/0<br>
 ip auth mode <span class="star">eigrp 100 md5</span><br>
 ip auth key-chain <span class="star">eigrp 100 EIGRP-SEC</span>
</div>
<p><small><span class="trap">RIP:</span> <code>ip rip auth mode md5</code></small></p>

<h2>📦 3. Route Summarization</h2>
<div class="code">
int g0/0<br>
ip summary-address <span class="star">eigrp 100 172.16.0.0 255.255.0.0</span>
</div>
<p><small><span class="trap">RIP:</span> No interface summary! Only <code>no auto-summary</code></small></p>
<h2>EIGRP Verification Commands Table</h2>

| Command                      | Purpose                          | Key Output Fields                          |
|------------------------------|----------------------------------|--------------------------------------------|
| `show ip eigrp neighbors`    | Check neighbor relationships     | H (Hold), Address, Interface, SRTT, Seq Num |
| `show ip eigrp topology`     | View topology table              | P/A (state), Successors, FD, Via path      |
| `show ip protocols`          | Protocol summary & K-values      | AS, Metric weights (K1-K6), Redistributing |
| `show ip route eigrp`        | EIGRP routes in routing table    | D (code), [AD/FD], Via IP, Interface       |
| `show ip eigrp interfaces`   | Interface EIGRP status           | Peers, Xmit Queue, SRTT, Pending Routes    |
| `debug eigrp packets`        | Live packet monitoring           | Hello, Update, Query packets (use sparingly)|
| `ping/traceroute [IP]`       | Test route usability             | Success %, RTT, Hops                        |


