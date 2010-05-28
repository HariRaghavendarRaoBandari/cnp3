
.. index:: Reference models

The reference models
####################

Given the growing complexity of computer networks, network researchers proposed during the 1970s reference models that allow to describe network protocols and services. The Open Systems Interconnection (OSI) model [Zimmermann80]_ was probably the most influential one. It was the basis for the standardisation work performed within the :term:`ISO` to develop global computer network standards. The reference model that we use in this book can be considered as a simplified version of the OSI reference model [#fiso-tcp]_.

.. index:: Five layers reference model

The five layers reference model
-------------------------------

Our reference model is divided in five layers as shown in the figure below.

.. figure:: png/intro-figures-026-c.png
   :align: center
   :scale: 50 

   The five layers of the reference model


Starting from the bottom, the first layer is the Physical layer. Two communicating devices are  linked through a physical medium. The physical medium is used to transfer and electrical or optical signal between the two devices. Different types of physical mediums are used in practice : 

 - `electrical cable`. Information can be transmitted over different types of electrical cables. The most common ones are twisted pairs that are used in the telephone network, but also in enterprise networks and coaxial cables. Coaxial cables are still used in cable TV networks, but not anymore in enterprise networks. 
 - `optical fiber`. Optical fibers are frequently used in public and enterprise networks with the distance is larger than one kilometer. 
 - `wireless`. In this case, a radio signal is used to encode the information being exchanged between the communicating devices. 

.. note:: Additional information about the physical layer will be added later.

An important point to note about the Physical layer is the service that it provides. This service is usually an unreliable connection-oriented service that allows the users of the Physical layer to exchange bits. The unit of information transfer in the Physical layer is the bit. The Physical layer service is unreliable because :

 - The Physical layer may change, e.g. due to electromagnetic interferences, the value of a bit being transmitted
 - the Physical layer may deliver `more` bits to the receiver than the bits sent by the sender
 - the Physical layer may deliver `fewer` bits to the receiver than the bits sent by the sender

The last two points may seem strange at first glance. When two devices are attached through a cable, how is it possible for bits to be created or lost on such a cable ? 

This is mainly due to the fact that the communicating devices use their own clock to transmit bits at a given bandwidth. Consider a sender having a clock that ticks one million times per second and sends one bit every tick. Every microsecond, the sender sends an electrical or optical signal that encodes one bit. The sender's bandwidth is thus 1 Mbps. If the receiver clock ticks exactly [#fsynchro]_ every microsecond, it will also deliver 1 Mbps to its user. However, if the receiver's clock is slightly faster (resp. slower), than it will deliver slightly more (resp. less) than one million bits every second.

.. sidebar:: Bandwidth

 In computer networks, the bandwidth achievable through the physical layer is always expressed in bits per second. A Mega bps is one million bits per second and a Giga bps is one billion bits per second. This is in contrast with memory specifications that are usually expressed in bytes (8 bits), KiloBytes ( 1024 bytes) or MegaBytes (1048576 bytes). Thus transferring one MByte through a 1 Mbps link lasts 1.048 seconds.


.. index:: Physical layer


.. figure:: png/intro-figures-027-c.png
   :align: center
   :scale: 50

   The Physical layer


The physical layer allows thus two or more entities that are directly attached to the same transmission medium to exchange bits. Being able to exchange bits is important because virtually any information can be encoded as a sequence of bits. Electrical engineers are used to process streams of bits, but computer scientists usually prefer to deal with higher level concepts. A similar issue arises with file storage. Storage devices such as hard-disks also store streams of bits. There are hardware devices that process the bit stream produced by a hard-disk, but computer scientists have designed filesystems to allow applications to easily access such storage devices. These filesystems are typically divided in several layers as well. Hard-disks store sectors of 512 bytes or more. Unix filesystems groups sectors in larger blocks that can contain data or inodes that represent the structure of the filesystem. Finally, applications manipulate files and directories that are translated in blocks, sectors and eventually bits by the operating system.

.. index:: Datalink layer
.. index:: frame


Computer networks use a similar approach and each layer provides a service that it built above the underlying layer and is closer to the needs of the applications. The `Datalink layer` builds on the service provided by the underlying physical layer. The `Datalink layer` allows two hosts that are directly connected through the physical layer to exchange information. The unit of information exchanged between two entities in the `Datalink layer` is a frame. A frame is a finite sequence of bits. Some `Datalink layers` user variable-length frames while others only use fixed-length frames. Some `Datalink layers` provide a connection-oriented service while others provide a connectionless service. Some `Datalink layers` provide a reliable delivery while others do not guarantee the correct delivery of the information.

An important point to note about the `Datalink layer` is that although the figure below indicates that two entities of the `Datalink layer` exchange frames directly, in reality this is slightly different. When the `Datalink layer` entity on the left needs to transmit a frame, it issues as many `Data.request` to the underlying `physical layer` as there are bits in the frame. The physical layer will then convert the sequence of bits in an electromagnetic physical that will be sent over the physical medium. The `physical layer` on the right side of the figure will decode the received signal, recover the bits and issue the corresponding `Data.indication` primitives to its `Datalink layer` entity. If there are not transmission errors, this entity will receive the frame sent earlier. 


.. figure:: png/intro-figures-028-c.png
   :align: center
   :scale: 50 

   The Datalink layer


.. index:: Network layer, packet

The `Datalink layer` allows directly connected hosts to exchange information, but it is often necessary to exchange information between hosts that are not attached to the same physical medium. This is the task of the `network layer`. The `network layer` is built above the `datalink layer`. The network layer entities exchange `packets`. A `packet` is a finite sequence of bytes. A packet usually contains information about its origin and its destination. A packet usually passes through several intermediate devices called routers on its way from its origin to its destination.

Different types of network layers can be implemented. The Internet uses an unreliable connectionless network layer service. Other networks have used reliable and unreliable connection-oriented network layer services.


.. figure:: png/intro-figures-029-c.png
   :align: center
   :scale: 50 

   The network layer

.. index:: Transport layer, segment

Most realisations, including the Internet, of the network layer do not provide a reliable service. However, many applications need to exchange information reliably and using the network layer service directly would be very difficult for them. Ensuring a reliable delivery of the data produced by applications is the task of the `transport layer`. `Transport layer` entities exchange `segments`. A segment is a finite sequence of bytes. A transport layer entity issues segments (or sometimes part of segments) as `Data.request` to the underlying network layer entity. 

There are different types of transport layers. The most widely used on the Internet are :term:`TCP` that provides a reliable connection-oriented bytestream transport service and :term:`UDP` that provides an unreliable connection-less transport service.


.. figure:: png/intro-figures-030-c.png
   :align: center
   :scale: 50 

   The transport layer

.. index:: Application layer

The upper layer of our architecture is the `Application layer`. It includes all the mechanisms and data structures that are necessary for the applications. We will use Application Data Unit (ADU) to indicate the data exchanged between two entities of the Application layer.

.. figure:: png/intro-figures-031-c.png
   :align: center
   :scale: 50 

   The Application layer

.. index:: TCP/IP reference model


The TCP/IP reference model
--------------------------

In contrast with OSI, the TCP/IP community did not spend a lot of effort at defining a detailed reference model and in fact the goals of the Internet architecture were only documented after TCP/IP had been deployed [Clark88]_. :rfc:`1122` that defines the requirements for Internet hosts mentions four different layers. Starting from the top, these are :

- the application layer
- the transport layer
- the internet layer which is equivalent to the network layer of our reference model
- the link layer which combines the functionalities of the physical and datalink layers.

Besides this difference in the lower layers, the TCP/IP reference model is very close to the five layers that we use throughout this document.

.. index:: OSI reference model

The OSI reference model
-----------------------

Compared to the five layers reference model explained above, the :term:`OSI` reference model defined in [X200]_ is divided in seven layers. The four lower layers are similar to the four lower layers described above. The OSI reference model refined the application layer by dividing it in three layers :

 - the Session layer. The Session layer contains the protocols and mechanisms that are necessary to organize and to synchronize the dialogue and manage the data exchange of presentation layer entities. While one of the main functions of the transport layer is to cope with the unreliability of the network layer, the session's layer objective is to hide the failure of transport-level connections to the upper layer higher. For this, the Session Layer provides services that allow to establish a session-connection, to support orderly data exchange (including mechanisms that allow to recover from the abrupt release of an underlying transport connection), and to release the connection in an orderly manner. 
 - the Presentation layer was designed to cope with the different ways of representing information on computers. There are many differences in the way computer store information. Some computers store integers as 32 bits field, others use 64 bits field and the same problem arises with floating point number. For textual information, this is even more complex with the many different character codes that have been used [#funicode]_. The situation is even more complex when considering the exchange of structured information such as records. To solve this problem, the Presentation layer contains provides for common representation of the data transferred. The :term:`ASN.1` notation was designed for the Presentation layer. 
 - the Application layers that contains the mechanisms that do not fit in neither the Presentation nor the Session layer. The OSI Application layer was itself further divided in several generic service elements. 

.. sidebar:: Where are the missing layers in TCP/IP reference model ?

 The TCP/IP reference places the Presentation and the Session layers implicitly in the Application layer. The main motivations for simplifying the upper layers in the TCP/IP reference model were pragmatic. Most Internet applications started as prototypes that evolved and were standardised later. Many of these applications assumed that they would be used to exchange information written in American English and for which the 7 bits US-ASCII character code was sufficient. This was the case for email, but as we'll see in the next chapter email was able to evolve to support different characters encodings. Some applications considered the different data representations explicitly. For example, :term:`ftp` contained mechanisms to convert a file from one format to another. On the other hand, many ISO specifications were developed by committees composed of people who did not all participate in actual implementations. ISO spent a lot of effort at analysing the requirements and defining a solution that meets all these requirements. Sometimes, the specification was so complex that it was difficult to implement it completely... 

.. The work within ISO and ITU on protocols and services was 

.. figure:: png/intro-figures-032-c.png
   :align: center
   :scale: 50 

   The seven layers of the OSI reference model


.. rubric:: Footnotes


.. [#unicode] There is now a rough consensus on using more and more the Unicode_ character format. Unicode can represent more than 100,000 different characters from the known written languages on Earth. Maybe one day all computers will only use Unicode to represent all their stored characters and Unicode could become the standard format to exchange characters, but we are not yet at this stage today. Even then, it would be necessary to decide which version of Unicode to use.

.. [#fiso-tcp] An interesting historical discussion of the OSI-TCP/IP debate may be found in [Russel06]_

.. [#fsynchro] Having perfectly synchronised clocks running at a high frequency is very difficult in practice. However, some physical layers introduce a feedback loop that allows the receiver's clock to synchronise itself automatically to the sender's clock. However, not all physical layers include this kind of synchronisation. 

.. include:: ../links.rst