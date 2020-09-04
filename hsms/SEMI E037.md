# SEMI E37-0298 HIGH-SPEED SECS MESSAGE SERVICES(HSMS) GENERIC SERVICES

## 1 Purpose

HSMS provides a means for independent manufacturers to produce implementations which can be connected and interoperate without requiring specific knowledge of one another.

HSMS is intended as an alternative to SECS E4(SECS-I) for applications where higher speed communication is needed or when a simple point-to-point topology is insufficient. SIMI E4(SECS-I) can still be used in applications where these and other attributes of HSMS are not required.

HSMS is also intended as an alternative to SEMI E13(SECS Message Services) for applications where TCP/IP is preferred over OSI.

It is intended that HSMS be supplemented by subsidiary standards which further specify details of its use or impose restrictions on its use in particular application domains.

## 2 Scope

Hight Speed SECS Message Services(HSMS) defines a communication interface suitable for the exchange of messages between computers in a semiconductor facotry.

## 3 Referenced Documents

### 3.1 *Semi Standards*

SEMI E4 -- SEMI Equipment Communication Standard 1 -- Message Transport (SECS-I)

SEMI E5 -- SEMI Equipment Communication Standard 2 -- Message Content (SECS-II)

### 3.2 *IETF Documents*

IETF RFC 791 -- Internet Protocol

IETF RFC 792 -- Internet Control Message Protocol

IETF RFC 793 -- Transmission Control Protocol

IETF RFC 1120 -- Requirements for Internet Hosts -- Communication Layers

IETF RFC 1340 -- Assigned Numbers. Note: This RFC supersedes RFC 820.

### 3.3 *POSIX Document*

IEEE POSIX P1003.12 -- Protocol Independent Interfaces (PII)



## 4 Terminology

***API*** -- Application Program Interface. In the case of TCP/IP, a set of programming conventions used by an application program to prepare for or invoke TCP/IP capabilities.

***communication failure*** -- A failure in the communication link resulting from a transition to the NOT CONNECTED state from SELECTED state. (See Section 9.)

***confirmed service (HSMS)*** -- An HSMS service requested by sending a message from the initiator to the responding entity which requires that completion of the service be indicated by a response message from the responding entity to the initiator.

***connection*** -- A logical linkage established on a TCP/IP LAN between two entities for the purpose of exchanging messages.

***control message*** -- An HSMS message used for the management of HSMS sessions between two entities.

***data message*** -- An HSMS message used for communication of application-specific data within an HSMS session. A Data Message can be a Primary Message or a Reply Message.

***entity*** -- An application program associated with an endpoint of a TCP/IP connection.

***header*** -- A 10-byte data element preceding every HSMS message.

***initiator (HSMS)*** -- The entity requesting an HSMS service. The initiator requests the service by sending an appropriate HSMS message.

***IP Address*** -- Internet Protocol Address. A logical address which uniquely identifies a particular attachment to a TCP/IP network.

***local entity*** -- Relative to a particular end point of a connection, the local entity is that entity associated with that endpoint.

***local entity-specific*** -- General qualifier to any procedure, option, issue, or other implementation matter which is not subject of this standard and left to the discretion of the individual supplier.

***message*** -- A complete unit of communication in one direction. An HSMS Message consists of the Message Length, Message Header, and the Message Text. An HSMS Message can be a Data Message or a Control Message.

***message length*** -- A 4-byte unsigned integer field specifying the length of a message in bytes.

***open transaction*** -- A transaction in progress.

***port*** -- An endpoint of a TCP/IP connection whose complete network address is specified by an IP Address and TCP/IP Port number.

***port number*** -- (or TCP port number). The address of a port within an attachment to a  TCP/IP network which can serve as an endpoint of a TCP/IP connection.

***primary message*** -- An HSMS Data Message with an odd numbered Function. Also, the first message of a data transaction.

***published port*** -- A TCIP/IP IP Address and Port nubmer associated with a particular entity (server) which that entity intends to use for receiving TCP/IP connection requests. An entity's published port must be known by remote entities intending to initiate connections.

***receiver***-- The HSMS Entity receiving a message.

***remote entity*** -- relative to a particular endpoint of a connection, the remote entity is the entity associated with the opposite endpoint of the connection.

***reply*** -- An HSMS Data Message with an even-numbered function. Also, the appropriate response to a Primary HSMS Data Message.

***responding entity (HSMS)*** -- The provider of an HSMS service. The responding entity receives a message from an initiator requesting the service. In the event of a confirmed service, the responding entity indicates completion of the requested service by sending an appropriate HSMS response message to the initiator of the request. In an unconfirmed service, the responding entity does not send a response message.

***session*** -- A relationship established between two entities for the purpose of exchanging HSMS messages.

***session entity*** -- An entity participating in an HSMS session.

***session ID*** -- A 16-bit unsigned integer which identifies a particular session between particular session entities.

***stream (TCP/IP)*** -- A sequence of bytes presented at one end of a TCP/IP connection for delivery to the other end. TCP/IP guarantees that the delivered sequence of bytes matches the presented stream. HSMS subdivides a stream into blocks of contiguous bytes - messages.

***T3*** -- Reply timeout in the HSMS protocol.

***T5*** -- Connect Separation Timeout in the HSMS protocol used to prevent excessive TCP/IP connect activity by providing a minimum time between the breaking, by an entity, of a TCP/IP connection or a failed attempt to establish one, and the attempt, by that same entity, to initiate a new TCP/IP connection.

***T6*** -- Control Timeout in the HSMS protocol which defines the maximum time an HSMS control transaction can remain open before a communications failure is considered to have occurred. A transaction is considered open from the time the initiator sends the required request message until the response message is received.

***T7*** -- Connection Idle Timeout in the HSMS protocol which defines the maximum amount of time which may transpire between the formation of a TCP/IP connection and the use of that connection for HSMS communications before a communications failure is considered to have occurred.

***T8*** -- Network Intercharacter Timeout in the HSMS protocol which defines the maximum amount of time which may transpire between the receipt of any two successive bytes of a complete HSMS message before a communications failure is considered to have occurred.

***TCP/IP*** -- Transmission Control Protocol/Internet Protocol. A method of communications which provides reliable, connection-oriented message exchange between computers within a network.

***TLI*** -- Transport Level Interface. One particular API provided by certain implementations of TCP/IP which provides a transport protocol and operating system independent definition of the use of any Transport Level protocol.

***transaction*** -- A Primary Message and its associated Reply message, if required. Also, an HSMS Control Message of the request (.req) type, and its response Control Message (.rsp), if required.

***unconfirmed service (HSMS)*** -- An HSMS service requested by sending a message from the initiator to the responding entity which requires no indication of completion from the responding entity.

## 5 HSMS Overview and State Diagram

High-Speed SECS Message Service (HSMS) defines a communication interface suitable for the exchange of messages between computers in a semiconductor factory using a TCP/IP environment. HSMS uses TCP/IP stream support, which provides reliable two way simultaneous transmission of streams of contiguous bytes. It can be used as a replacement for SECS-I communication as well as other more advanced communications environments.

The procedure for HSMS communications parallels the more familiar SECS-I communications it replaces. The following steps are followed for any communications(HSMS or otherwise):

1. Obtain a communications link between two entities. In SECS-I, this is the RS232 wire physically connecting host and equipment. In HSMS, the link is a TCP/IP connection obtained by the standard TCP/IP connect procedure. Note that the abstract term "entity" is used instead of "host" or "equipment". This is because, while HSMS is used for SECS-I replacement, it has more general applications as well. In a SECS-I replacement application, the "host" is an "entity" and the "equipment" is an "entity".

2. Establish the application protocol conventions to be used for exchanging data messages between two entities. For SECS-I, this step is implicit in the fact that semiconductor equipment is physically connected on the two ends of the wire: the protocol is SECS-II.

   In the case of HSMS, the communications link is a dynamically established TCP/IP connection on a physical link which may be shared with many other TCP/IP connections using protocols other than HSMS or connections using non TCP/IP protocols. HSMS adds a message exchange (called the Select procedure) which is used to confirm to both entities that the particular TCP/IP connection is to be used exclusively for HSMS communications.

3. Exchange Data. This is the normal intended purpose of the communications link. In both SECS-I and HSMS, the procedure is to exchange SECS-II encoded messages for the control of semiconductor equipment and/or processes. Data exchange normally continues until one or both of the entities are taken off-line for equipment-specific purposes, such as maintenance.

4. Formally end communications. In SECS-I, there is no formal requirement here; the equipment to be taken off-line stops communicating.

   In HSMS, a message exchange (either the "bilateral" Deselect procedure or the "unilateral" separate procedure) is used for both parties to confirm that the TCP/IP connection is no longer needed for HSMS communications.

5. Break the communications link. In SECS-I, this is done by physically unplugging the host or equipment from the communication cable, which only occurs during repair or physical reconfiguration of the factory network environment.

   In HSMS, since it uses the dynamic connection environment of TCP/IP, the TCP/IP connection is logically broken via a release or a disconnect procedure without any physical disconnect from the network medium.

Two additional procedures, of a diagnostic nature, are supported in HSMS, which are generally not required by a simple SECS-I link or a SECS-I direct replacement. These follow:

1. Linktest. This procedure provides a simple confirmation of connection integrity.
2. Reject. Because HSMS is intended to be extended to protocols other than just SECS-II (by means of subsidiary standards), it is possible that two entities can be connected (due to a configuration error) which use incompatible subsidiary standards. Also, during initial implementation, incorrect message types may be sent, or they may be sent out of order due to software bugs. The reject procedure is used to indicate such an occurrence.

### 5.1 *HSMS Connection State Diagram*

The HSMS state machine is illustrated in the diagram below. The behavior described in this diagram defines the basic requirements of HSMS: subsidiary standards may further extend these or other states.

![](https://raw.githubusercontent.com/Dualm/MdPics/master/hsms/state.png?token=ABTQXMIE5NHCIUN3O25BJ7C7KCL3U)

### 5.2 *State Descriptions*

**5.2.1 *NOT CONNECTED*** 

The entity is ready to listen for or initiate TCP/IP connections but either has not yet established any connections or all previously established TCP/IP connections have been terminated.

**5.2.2 *CONNECTED*** 

A TCP/IP connection has  been established. This state has two substates, NOT SELECTED and SELECTED.

**5.2.2.1 *NOT SELECTED*** 

A substate of CONNECTED in which no HSMS session has been established or any previously established HSMS session has ended.

**5.2.2.2 *NOT SELECT*** 

A substate of CONNECTED in which at least one HSMS session has been established. This is the normal "operating" state of HSMS: data messages may be exchanged in this state. It is highlighted by a heavy outline in the state diagram.

### 5.3 *State Transition Table*

<table>
	<tr>
    	<th><i>#</i></th>
        <th><i>Current State</i></th>
        <th><i>Trigger</i></th>
        <th><i>New State</i></th>
        <th><i>Actions</i></th>
        <th><i>Comment</i></th>
    </tr>
    <tr>
    	<td>1</td>
        <td>...</td>
        <td>Local entity-specific preparation for TCP/IP communication.</td>
        <td>NOT CONNECTED</td>
        <td>Local entity-specific</td>
        <td>Action depends on connection procedure to be used: active or passive.</td>
    </tr>
    <tr>
    	<td>2</td>
        <td>NOT CONNECTED</td>
        <td>A TCP/IP connection is established for HSMS communication.</td>
        <td>CONNECTED-NOT SELECTED</td>
        <td>Local entity-specific</td>
        <td>none</td>
    </tr>
    <tr>
    	<td>3</td>
        <td>CONNECTED</td>
        <td>Breaking of TCP connection.</td>
        <td>NOT CONNECTED</td>
        <td>Local entity-specific</td>
        <td>See Section 6.4</td>
    </tr>
    <tr>
    	<td>4</td>
        <td>NOT SELECTED</td>
        <td>Successful completion of HSMS Select Procedure.</td>
        <td>SELECTED</td>
        <td>Local entity-specific</td>
        <td>HSMS communication is now fully established: data message exchange is permitted.</td>
    </tr>
    <tr>
    	<td>5</td>
        <td>SELECTED</td>
        <td>Successful completion of HSMS Deselect or Separate</td>
        <td>NOT SELECTED</td>
        <td>Local entity-specific</td>
        <td>This transition normally indicates the end of HSMS communication and so an entity would immediately proceed to break the TCP/IP connection (transition 3 above)</td>
    </tr>
    <tr>
    	<td>6</td>
        <td>NOT SELECTED</td>
        <td>T7 Commection Timeout</td>
        <td>NOT CORRECTED</td>
        <td>Local entity-specific</td>
        <td>per Section 9.2.2</td>
    </tr>
</table>

## 6 Use of TCP/IP

### 6.1 *TCP/IP API*

The specification of a required TCP Application Program Interface (API) for use in implementations is outside the scope of HSMS. A given HSMS implementation may use any TCP/IP API -- sockets, TLI(Transport Layer Interface), etc. -- appropriate to the intended hardware and software platform, as long as it provides interoperable TCP/IP streams protocol on the network.

The appendix contains examples of the TCP/IP procedures referenced in this standard and sample scenarios using both the TLI POSIX standard 1003.12) and the popular BSD socket model for TCP/IP communication.

### 6.2 *TCP/IP Network Addressing Conventions*

**6.2.1 *IP Addresses*** 

Each physical TCP/IP connection to a given Local Area Network (LAN) must have a unique IP Address. IP Addresses must be assignable at installation time, and an HSMS implementation cannot select a fixed IP Address. A typical IP Address is 192.9.200.1.

**6.2.2 *TCP Port Numbers*** 

A TCP Port Number can be considered as an extension of the IP Address.

HSMS implementations should allow configuring TCP Port to the full range of the TCP/IP implementation used. A typeical TCP Port Number is 5000.

Conventions have been established for selecting TCP Port Numbers which are outside the scope of the HSMS protocol. Consult RFC 793, Transmission Control Protocol (TCP) in Section 3.

### 6.3 *Establishing a TCP/IP Connection*

**6.3.1 *Connect Modes*** 

The procedures for establishing a TCP/IP connection are defined in RFC 793. However, not all the procedures defined by RFC 793 are supported by commonly available APIs. In particular, while RFC 793 permits both entities to initiate the connection simultaneously, this feature is rarely supported in available APIs. Therefore,  HSMS restricts an entity to one of the following modes:

- **Passive Mode**. The Passive mode is used when the local entity listens for and accepts a connect procedure initiated by the Remote Entity.
- **Active Mode**. The Active mode is used when the connect procedure is initiated by the Local Entity.

![](https://raw.githubusercontent.com/Dualm/MdPics/master/hsms/Connection.png?token=ABTQXMLFIPOR7JWKGMFNUO27KCU2K)

The appendix provides an example of how an entity may operate alternatively in the active and passive modes to achieve greater flexibility in establishing communication.

**6.3.2 *Passive Mode Connect Procedure*** 

The procedure followed by the Passive Local Entity is defined in RFC 793. It is summarized as follows:

1. Obtain a connection endpoint and bind it to a published port.
2. Listen for an incoming connect request to the published port from a remote entity.
3. Upon receipt of a connect request, acknowledge it and indicate acceptance of the connection. At this point, the connect procedure has completed successfully, and the CONNECTED state is entered (Section 5).

These procedures are carried out through the API of the local entity's implementation of TCP/IP. The appendices provide the API-specific procedures for the above steps using both TLI and BSD.

Note: A Failure may occur during the above steps. The reason for failure may be local entity-specific or may be due to a lack of any connect request after a local entity-specific timeout. The action to be taken (for example: return to step 1 to retry) is a local entity-specific issue. 

Note: See Section 9, Special Considerations, for issues relating to multiple connection requests to the same passive mode entity.

**6.3.3 *Active Mode Connect Procedure*** 

The procedure followed by the Active Local Entity is defined in RFC 793. It is summarized as follows:

1. Obtain a connection endpoint.
2. Initiate a connection to the published port of a passive mode remote entity.
3. Wait for the receipt of the acknowledge and the acceptance of the connect request from the remote entity. Receipt of the acceptance from the remote entity indicates successful completion of the connect procedure, and the CONNECTED state is entered (Section 5).

These procedures are carried out through the API of the local entity's implementation of TCP/IP. The appendix provides the API-specific procedures for the above steps using both TLI and BSD.

Note: A failure may occur during the above steps. The reason for failure may be local entity-specific or may be due to a lack of any accept message after a local entity-specific timeout. The action to be taken is a local entity-specific issue. If, however, the local entity intends to retry the connection, it should do so subject to the T5 connect separation timeout (see "Special Considerations").

### 6.4 *Terminating a TCP/IP Connection* 

Connection termination is the logical inverse of Connection establishment. From the Local Entity's perspective, a TCP/IP connection may be broken at any time. However, HSMS only permits termination of the connection when the connection is in the NOT SELECTED substate of the CONNECTED state.

The procedures for termination of a connection are defined in RFC 793. Either entity may initiate termination of the connection. The NOT CONNECTED state is entered, indicating the end of HSMS communications. The appendix illustrates the procedures for both release and disconnect using the TLI and BSD API.

## 7 HSMS Message Exchange Procedures

HSMS defines the procedures for all message exchange between entities across the TCP/IP connection established according to the procedures in the previous section. As explained in the overview, once the connection is established, the two entities establish HSMS communications with the Select procedure. Then data messages may be exchanged in either direction at any time. When the entities wish to end HSMS communications, the Deselect or Separate procedure is used to end HSMS communications.

### 7.1 *Sending and Receiving HSMS Messages* 

All HSMS procedures involve the exchange of HSMS messages. These messages are sent and received as TCP/IP streams using the previously established TCP/IP connection at standard priority. In particular, the use of "Urgent" data is not supported under HSMS (see RFC 793 for more information on send and receive procedures).

The appendix gives examples of sending and receiving HSMS messages using both TLI and BSD socket APIs.

### 7.2 *Select Procedures*

The Select procedure is used to establish HSMS communications on a TCP/IP connection using the Select.req and Select.rsp messages in a control transaction.

![](https://raw.githubusercontent.com/Dualm/MdPics/master/hsms/select%20control%20transaction.png?token=ABTQXMM3T2EB64GNFBXG5RS7KGLCE)

Although HSMS permits Select at any time in the CONNECTED state, subsidiary standards may further require the connection to be in the NOT SELECTED substate (see "Special Considerations").

**7.2.1 *Initiator Procedure*** 

The procedure followed by the initiator is as followers.

1. The initiator of the select procedure sends the Select.req message to the responding entity.
2. If the initiator receives a Select.rsp with a Select Status of 0, The HSMS Select procedure completes successfully and the SELECTED state is entered (see Section 5).
3. If the initiator receives a Select.rsp with a non-zero Select Status, the Select completes unsuccessfully (no state transitions).
4. If the T6 timeout expires in the initiator before receipt of a Select.rsp, it is considered a communications failure (see "Special Considerations").

**7.2.2 *Responding Entity Procudure*** 

The procedure followed by the responding entity is as follows.

1. The responding entity receives the Select.req.
2. If the responding entity is able to accept the select, it transmits the Select.rsp with a Select status of 0. The HSMS Select Procedure for the responding entity is successfully completed, and the SELECTED state is entered (see Section 5).
3. If the responding entity is unable to permit the select, it transmits the Select.rsp with a non-zero Select Status. The HSMS Selct Procedure for the responding entity completes unsuccessfully (no state transitions).

**7.2.3 *Simultaneous Select Procedures*** 

If the subsidiary standards do not restrict the use of the Select, it is possible that both entities simultaneously initiate Select Procedures with identical SessionID's. In such a case, each entity will accept the other entity's select request by responding with a Select.rsp.

![](https://raw.githubusercontent.com/Dualm/MdPics/master/hsms/simultaneous%20select%20transaction.png)

### 7.3 *Data Procedure*

HSMS data messages may be initiated by either entity as long as the connection is in the SELECTED state. Recipte of a data message when not in the SELECTED state will result in a reject procedure (see Section 7.7).

Data messages may be further defined as part of a data transaction as either a "Primary" or "Reply" data message. In a data transaction, the initiator of the transaction sends a primary message to the responding entity. If the Primary message indicates that a reply is expected, a Reply message is sent by the responding entity in response to the Primary.

The following types of Data Transactions are supported:

1. Primary Message with reply expected and the associated Reply Message.
2. Primary Message with no reply expected.

![](https://raw.githubusercontent.com/Dualm/MdPics/master/hsms/data%20message%20transaction.png)

The specific procedures for these transactions are determined by the application layer and are subject to other standards (for example, E5 and E30 for GEM equipment using SECS-II encoded messages).

The applicable upper layer standard is identified by the message type. The type is determined from the specific format defined in Section 8. The normal type for HSMS messages is SECS-II text. Also refer to "Special Considerations" concerning the T3 Reply Timeout.

### 7.4 *Deselect Procedure*

The Deselect procedure is used to provide a graceful end to HSMS communication for an entity prior to breaking the TCP/IP connectoin. HSMS requires that the connection be in the SELECTED state. The procedure is as follows.

![](https://raw.githubusercontent.com/Dualm/MdPics/master/hsms/deselect%20control%20transaction.png)

**7.4.1 *Initiator Procedure***

1. The initiator of the Deselect procedure sends the Deselect.req message to the responding entity.
2. If the initiator receives a Deselect.rsp with a Deselect Status of 0, its Deselect procedure teeminates successfully. The NOT SELECTED state is entered (see Section 5).
3. If the initiator receives a Deselect.rsp with a non-zero Deselect Statue, its Deselect procedure terminates unsuccessfully. No state change occurs.
4. If the T6 timeout expires in the initiator before receipt of a Deselect.rsp, it is considered a communications failure (see "Special Considerations").

**7.4.2 *Responding Entity Procedure***

1. The responding entity receives the Deselect.req message.
2. If the responding entity is in the SELECTED state, and if is it able to permit the Deselect, it responds using the Deselect.rsp with a zero response code. The responding entity's Deselect procedure completes successfully. The NOT SELECTED state is entered (see Section 5).
3. If the responding entity is unable to permit the Deselect, either because it is not in the SELECTED state or because local conditions do not permit the Deselect, it responds using the Deselect.rsp with a non-zero response code. The responding entity's Deselect procedure terminates unsuccessfully. No state change occurs.

**7.4.3 *Simultaneous Deselect Procedures***

If the subsidiary standards do not restrict the use of the Deselect, it is possible that both entities simultaneously initiate Deselect Procedures with identical SessionID's. In such a case, each entity will accept the other entity's Deselect request by responding with the deselect.rsp.

![Simultaneous Deselect Transactions](https://raw.githubusercontent.com/Dualm/MdPics/master/hsms/simultaneous%20deselect%20transactions.png)

### 7.5 *Linktest Procedure*

The Linktest is used to determine the operational integrity of TCP/IP and HSMS communications. Its use is valid anytime in the CONNECTED state.

![Linktest Control Transaction](https://raw.githubusercontent.com/Dualm/MdPics/master/hsms/linktest%20control%20transaction.png)

**7.5.1 *Initiator Procedure***

1. The initiator of the Linktest procedure sends the Linktest.req message to the responding entity.
2. If the initiator receives a Linktest.rsp within the T6 timeout, the Linktest is successfully completed.
3. If the T6 timeout expires in the initiator before receipt of a Linktest.rsp, it is considered a communications failure (see "special Considerations").

**7.5.2 *Responding Entity Procedure***

1. The responding entity receives the Linktest.req from the initiator.
2. The responding entity sends a Linktest.rsp.

### 7.6 *Separate Procedure*

The Sparate procedure is used to abruptly terminate HSMS communication for an entity prior to breaking the TCP/IP Connection. HSMS requies that the connection be in the SELECTED state when using Separate. The responding entity does not send a response and is required to terminate communications regardless of its local state. The procedure is as follows.

![](https://raw.githubusercontent.com/Dualm/MdPics/master/hsms/separate%20control%20transaction.png)

**7.6.1 *Initiator Procedure***

1. The initiator of the select procedure sends the Separate.req message to the responding entity. The initiator's Separate procedure completes successfully.The NOT SELECTED state is entered (see Section 5).

**7.6.2 *Responding Entity Procedure***

1. The responding entity receives the Separate.req from the initiator.
2. If the responding entity is in the SELECTED state, its Separate procedure completes successfully.
3. If the responding is not  in the SELECTED state, the Separate.req is ignored.

### 7.7 *Reject Procedure*

The Reject procedure is used in response to an otherwise valid HSMS message received in an inappropriate context. Supporting the reject procedure can provide useful diagnostic information during the development of a distributed application using HSMS. The procedure is as follows:

![](https://raw.githubusercontent.com/Dualm/MdPics/master/hsms/reject%20transaction.png)

**7.7.1 *Initiator (Sender of Inappropriate Message) Procedure***

1. The initiator of the inappropriate message, upon receiving the Reject.req, takes appropriate action (local entity-specific).

**7.7.2 *Responding Entity Procedure***

1. The entity receiving the inappropriate message responds with a Reject.req message.

HSMS requires the reject procedure for the receipt of a data message in the NOT SELECTED state, or the receipt of a message whose SType or PType (see next section: Message Format) is not defined for the entity receiving the message. Subsidiary standards may define other conditions which require the Reject Procedure. In general, receipt of a reject message is an inidcation of an improperly configured system or a software programming error.

## 8 HSMS Message Format

This section defines the detailed format of the messages used by the procedure in the previous section.

### 8.1 *General Message Format*

**8.1.1 *Byte Structure***

Within HSMS, a byte contains eight(8) bits. The bits in a byte are numbered from Bit 7 (most significant) to Bit 0 (least significant).

**8.1.2 *Message Format***

An HSMS Message is transmitted as a single contiguous stream of bytes in the following order:

<table>
    <tr>
        <th colspan=2 align="center">HSMS Message Format</th>
    </tr>
    <tr>
    	<th>Number of Bytes</th>
        <th>Description</th>
    </tr>
    <tr>
    	<td>4 Bytes</td>
        <td>Message Length. MSB First. Specifies the number of bytes in the Message Header plus the Message Text.</td>
    </tr>
    <tr>
    	<td>10 Bytes</td>
        <td>Message Header.</td>
    </tr>
    <tr>
    	<td>0-n Bytes</td>
        <td>Message Text. Format is further specified by PType field of message header</td>
    </tr>
</table>

**8.1.3 *Message Length***

Message Length is a four-byte unsigned integer value which specifies the length in bytes of the Message Header plus the Message Text. Message Length is transmitted most significant byte (MSB) first and least significant byte (LSB) last.

The minimum possible Message Length is 10 (Header only). The maximum possible Message Length is implementation-specific.

**8.1.4 *Message Header***

The Message Header is a ten-byte field. The bytes in the header are numbered from byte 0 (first byte transmitted) to byte 9 (last byte transmitted). The format of the Message Header is as follows:

<table>
    <tr>
        <th align="center" colspan=2>HSMS Message Header</th>
    </tr>
    <tr>
    	<th>Bytes</th>
        <th>Description</th>
    </tr>
    <tr>
    	<td>0-1</td>
        <td>Session ID(Device ID</td>
    </tr>
    <tr>
    	<td>2</td>
        <td>Header Byte 2</td>
    </tr>
    <tr>
    	<td>3</td>
        <td>Header Byte 3</td>
    </tr>
    <tr>
        <td>4</td>
        <td>PType</td>
    </tr>
    <tr>
    	<td>5</td>
        <td>SType</td>
    </tr>
    <tr>
    	<td>6-9</td>
        <td>System Bytes</td>
    </tr>
</table>

The physical byte order is designed to correspond as closely as possible to the SECS-I header.

**8.1.4.1 *Session ID*** 

Session ID is a 16-bit unsigned integer value, which occupies bytes 0 and 1 of the header (byte0 is MSB, 1 is LSB). Its purpose is to provide an association by reference between control messages (particularly Select and Deselect) and subsequent data messages. It is the role of HSMS subsidiary standards to specify this association further.

**8.1.4.2 *Header Byte 2***

This header byte is used in different ways for different HSMS messages. For Control Messages (see SType, below) it contains zero or a status code. For a Data Message whose PType (see below) = 0, it contains the W-bit and SECS Stream. For a Data Message with PType not equal to 0, see "Special Considerations".

**8.1.4.3 *Header Byte 3***

This header byte is used in different ways for different HSMS messages. For Control Messages, it contains zero or a status code. For a Data Message whose PType (see below) = 0, it contains the SECS Function. For a Data Message with PType not equal to 0, see "Special Considerations".

**8.1.4.4 *PType***

PType (Presentation Type) is an 8-bit unsigned integer value which occupies byte 4 of the header. PType is intended as an enumerated type defining the presentation layer message type: how the Message Header and Message Text are encoded. Only PType = 0 is defined by HSMS to mean SECS-II message encoding. For non-zero PType values, see "Special Considerations".

<table>
    <tr>
    	<th align="center" colspan=2>PType</th>
    </tr>
    <tr>
    	<th>Value</th>
        <th>Description</th>
    </tr>
    <tr>
    	<td>0</td>
        <td>SECS-II Encoding</td>
    </tr>
    <tr>
        <td>1-127</td>
        <td>Reserved for subsidiary standards</td>
    </tr>
    <tr>
    	<td>128-255</td>
        <td>Reserved, not used</td>
    </tr>
</table>

**8.1.4.5 *SType***

SType (Session Type) is one-byte unsigned integer value which occupies header byte 5.

SType is an enumerated type identifying whether this message is an HSMS Data Message (value=0) or one of the HSMS Control Messges (other). Those values not explicitly defined in the table are addressed in "Special Considerations".

<table>
    <tr>
        <th align="center" colspan=2>SType</th>
    </tr>
    <tr>
    	<th>Value</th>
        <th>Description</th>
    </tr>
    <tr>
    	<td>0</td>
        <td>Data Message</td>
    </tr>
    <tr>
    	<td>1</td>
        <td>Select.req</td>
    </tr>
    <tr>
    	<td>2</td>
        <td>select.rsp</td>
    </tr>
    <tr>
    	<td>3</td>
        <td>Deselect.req</td>
    </tr>
    <tr>
    	<td>4</td>
        <td>Deselect.rsp</td>
    </tr>
    <tr>
    	<td>5</td>
        <td>Linktest.req</td>
    </tr>
    <tr>
    	<td>6</td>
        <td>Linktest.rsp</td>
    </tr>
    <tr>
    	<td>7</td>
        <td>Reject.req</td>
    </tr>
    <tr>
    	<td>8</td>
        <td>(not used)</td>
    </tr>
    <tr>
    	<td>9</td>
        <td>Separate.req</td>
    </tr>
    <tr>
    	<td>10</td>
        <td>(not used)</td>
    </tr>
    <tr>
    	<td>11-127</td>
        <td>Reserved for subsidiary standards</td>
    </tr>
    <tr>
    	<td>128-255</td>
        <td>Reserved, not used</td>
    </tr>
</table>

**8.1.4.6 *System Bytes***

System Bytes is a four-byte field occupying header bytes 6-9. System Bytes is used to identify a transaction uniquely among the set of open transactions.

***Uniqueness*** -- The System Bytes of a Primary Data Message, Select.req, Deselect.req, or Linktest.req message must be unique from those of all other currently open transactions initiated from the same end of the connection. They must also be unique from those of the most recently completed transaction.

***Reply Message*** -- The System Bytes of a Reply Data message must be the same as those of the corresponding Primary Message. The System Bytes of a Select.rsp, Deselect.rsp, or Linktest.rsp must be the same as those of the respective ".req" message.

## 8.2 *HSMS Message Formats by Type*

The specific interpretation of the header bytes in an HSMS message is dependent on the specific HSMS message type as defined by the value of the SType field. The complete set of messages defined is summarized in the table below, shown for PType = 0 (SECS-II message format).

<table>
    <tr>
    	<th align="center" colspan=8>HSMS Message Format Summary</th>
    </tr>
    <tr>
    	<th></th>
        <th align="center" colspan=6>Message Header</th>
        <th></th>
    </tr>
    <tr>
        <th>Message Type</th>
        <th>Bytes 0-1 SessionID</th>
        <th>Byte 2</th>
        <th>Byte 3</th>
        <th>Byte 4 PType</th>
        <th>Byte 5 SType</th>
        <th>Bytes 6-9 System Bytes</th>
        <th>Message Text</th>
    </tr>
    <tr>
        <td>Data Message</td>
        <td>*</td>
        <td>W-Bit and SECS Stream</td>
        <td>SECS Function</td>
        <td>0</td>
        <td>0</td>
        <td>Primary: Unique              Reply: Same as Unique</td>
        <td>Text</td>
    </tr>
<tr>
	<td>Select.req</td>
    <td>*</td>
    <td>0</td>
    <td>0</td>
    <td>0</td>
    <td>1</td>
    <td>Unique</td>
    <td>none</td>
</tr>
<tr>
    <td>Select.rsp</td>
    <td>Smae as .req</td>
    <td>0</td>
    <td>Select Status</td>
    <td>0</td>
    <td>2</td>
    <td>Same as .req</td>
    <td>none</td>
</tr>
<tr>
	<td>Deselect.req</td>
    <td>*</td>
    <td>0</td>
    <td>0</td>
    <td>0</td>
    <td>3</td>
    <td>Unique</td>
    <td>none</td>
</tr>
<tr>
	<td>Deselect.rsq</td>
    <td>Same as .req</td>
    <td>0</td>
    <td>Deselect Status</td>
    <td>0</td>
    <td>4</td>
    <td>Same as .req</td>
    <td>none</td>
</tr>
<tr>
	<td>Linktest.req</td>
    <td>0xFFFF</td>
    <td>0</td>
    <td>0</td>
    <td>0</td>
    <td>5</td>
    <td>Unique</td>
    <td>none</td>
</tr>
<tr>
	<td>Linktest.rsp</td>
    <td>0xFFFF</td>
    <td>0</td>
    <td>0</td>
    <td>0</td>
    <td>6</td>
    <td>Same as .req</td>
    <td>none</td>
</tr>
<tr>
	<td>Reject.req</td>
    <td>same as message being rejected</td>
    <td>PType or SType of message being rejected</td>
    <td>Reason Code</td>
    <td>0</td>
    <td>7</td>
    <td>Same as message being rejected</td>
    <td>none</td>
</tr>
<tr>
	<td>Separate.req</td>
    <td>*</td>
    <td>0</td>
    <td>0</td>
    <td>0</td>
    <td>9</td>
    <td>Unique</td>
    <td>none</td>
</tr>
</table>

*: Indicates further specification by subsidiary standards.

**8.2.1 *SType=0: Data Message***

An HSMS message with SType = 0 is used by the HSMS Data procedure to send a Data message, either Primary or Reply. The message format is as follows:

HSMS Message Length is always 10 (the length of the header alone) or greater.

The HSMS Message Header is as follows:

- Session ID -- As described above. Specific value subject to subsidiary standards.

- Header Byte 2 -- For message with PType value = 0 (SECS-II), header byte 2 is formatted as shown below.

  <table>
      <tr>
      	<td>7</td>
          <td>6 </td>
          <td align="right">0</td>
      </tr>
      <tr>
      	<td>W-Bit</td>
          <td align="center" colspan=2>Stream</td>
      </tr>
  </table>

  The most significant bit (bit 7) of Header Byte 2 is the W-Bit. In a Primary Message, the W-Bit indicates whether the Primary Message expects a Reply message. A Primary Message which expects a Reply should set the W-Bit to 1. A Primary Message which dose not expect a Reply should set the W-Bit to 0. A Reply Message shoutld always set the W-Bit to 0. The low-order 7 bits (bits 6-0) of header Byte 2 contain the SECS Stream for the message. The Stream is a 7-bit unsigned integer value, which identifies a major topic of the message ,and its use is defined within SEMI E5 (SECS-II).

- Header Byte 3 -- For messages whose PType value=0, header Byte 3 contains the SECS Function for the message. The Function is an 8-bit unisigned integer value which identifies a minor topic of the message (within the Stream), and its use is defined within SEMI E5 (SECS-II). The least significant bit (bit 0) of the Function defines whether the Data Message is Primary or Reply; the value 1 indicates Primary and the value 0 indicates Reply.

- PType -- Set PType = 0 for SECS-II messages.

- SType -- 0

- System Bytes -- Form PType=0 (SECS-II), the following definition applies. For a Primary Message, System Bytes contain a value uniquely identifying this transaction from all other open transactions initiated from the same end of the Connection. For a Reply Message, System Bytes contain the same value as the corresponding Primary Message.

The HSMS Message Text contains the text of the Data Message (if any), formatted as specified by the PType field. For PType = 0, the text will be formatted as SECS-II messages.

Note: Some Data messages consist of header only, with no text.

**8.2.2 *SType=1: Select.req***

An HSMS message with SType 1 is a "Select Request" Control Message, which is used by the initiator of the procedure for establishing HSMS communications. The message format is as follows:

Message Length is always 10 (Header only).

The HSMS Message Header is as follows:

- Session ID -- As described above. Specific value subject to subsidiary standards.
- Header Byte 2 = 0
- Header Byte 3 = 0
- PType = 0.
- SType = 1.
- System Bytes -- A unique value among open transactions.

**8.2.3 *SType=2: Select.rsp*** 

An HSMS message with SType 2 is a "Select.rsp" Control Message, used as the response to a Select.req Control Message in the procedure for establishing HSMS communications. The message format is as follows:

Message Length is always 10 (Header only).

The HSMS Message Header is as follows:

- Session ID -- must be equal to the value of the session ID in the corresponding Select.req.
- Header Byte 2 = 0
- header Byte 3 -- Select Status. A code of zero indicates success of the Select operation. A non-zero code indicates failure.



