```
<?xml version="1.0" encoding="ISO-8859-1" ?>

<!-- IMS Registration Scenario -->

<scenario name="IMS Registration">

  

  <!-- Send REGISTER Request -->

  <send>

    <![CDATA[

    REGISTER sip:[field1] SIP/2.0

    Via: SIP/2.0/[transport] [local_ip]:[local_port];rport;branch=[branch]

    Route: <sip:pcscf.[field1];lr>

    Max-Forwards: 70

    From: <sip:[field0]@[field1]>;tag=[call_number]

    To: <sip:[field0]@[field1]>

    Call-ID: [call_id]

    CSeq: 1 REGISTER

    Contact: sip:[field0]@[local_ip]:[local_port];transport=[transport]>

    Expires=3600

    Allow: PRACK, INVITE, ACK, BYE, CANCEL, UPDATE, INFO, SUBSCRIBE, NOTIFY, REFER, MESSAGE, OPTIONS

    Content-Length:  0

  

    ]]>

  </send>

  

    <recv response="100">

  </recv>

  
  

  <!-- Expect 401 Unauthorized Response -->

  <recv response="401" auth="true">

    <action>

      <!-- Extract nonce for authentication -->

      <!-- <assign var="nonce" expr="hdr_www_authenticate[nonce=(.*?),]"/> -->

      <ereg regexp="hdr_WWW-Authenticate[nonce=(.*?),]" search_in="msg" check_it="true" assign_to="1" />

    </action>

  </recv>

  

  <!-- Send REGISTER with Authentication -->

  <send>

    <![CDATA[

    REGISTER sip:[field1] SIP/2.0

    Via: SIP/2.0/[transport] [local_ip]:[local_port];rport;branch=[branch]

    Route: <sip:pcscf.[field1];lr>

    Max-Forwards: 70

    From: <sip:[field0]@[field1]>;tag=[call_number]

    To: <sip:[field0]@[field1]>

    Call-ID: [call_id]

    CSeq: 2 REGISTER

    Contact: <sip:[field0]@[local_ip]:[local_port];transport=[transport]>

    Expires=3600  

    Authorization: Digest username="[field0]", realm="[field1]", nonce="[$1]", uri="sip:[field1], response="83b51fd424541414964ff64c571e3614"

    Allow: INVITE, ACK, CANCEL, OPTIONS, BYE, REFER, NOTIFY, MESSAGE, SUBSCRIBE, INFO

    Supported: path

    Content-Length: 0

  

    ]]>

  </send>

<!--  -->

    <recv response="100">

  </recv>

  

  <!-- Expect 200 OK Response -->

  <recv response="200">

    <action>

      <!-- Registration successful -->

      <log message="Registration successful for user [field0]@[local_ip]:[local_port]"/>

    </action>

  </recv>

  

</scenario>
```