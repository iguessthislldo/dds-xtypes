# dds-xtypes
Validation of interoperability  of products compliant with [OMG DDS-XTYPES standard](http://www.omg.org/spec/DDS-XTypes/). This is considered one of the core [DDS Specifications](http://portals.omg.org/dds/omg-dds-standard/). See http://portals.omg.org/dds/ for an overview of DDS.

The executables in this repository test communication between *DDS DataWriters* and *DDS DataReaders* using different "versions" of a data type.  The goal is that the compatibility rules are implemented in compliance with the **OMG DDS-XTYPES** specification.

These tests can be used to validate compliance with **OMG DDS-XTYPES** each DDS implementation separately. The same executables can also be used to test interoperability between different DDS implementations. E.g. between *RTI Connext DDS* and *TwinOaks CoreDX DDS*.

The test uses 25 different "versions" of a data-type. These variations are obtained by adding, removing or reordering different attributes. They are also obtained by declaring different extensibility kinds for the data type (Final, Extensible [the default], and Mutable). These types are declared in the *ShapeTypes.idl* file or the equivalent *ShapeTypes.xml* file. The 25 types defined there are:

Types with default extensibility  | Types with final extensibility  | Types with extensible extensibility  | Types with mutable extensibility  | Types with mutable extensibiity specifying explicit ID 
------------- | ----------- | ---------------- | ------------- | ------------------------ 
Shape1Default | Shape1Final | Shape1Extensible | Shape1Mutable | Shape1MutableExplicitID
Shape2Default | Shape2Final | Shape2Extensible | Shape2Mutable | Shape2MutableExplicitID 
Shape3Default | Shape3Final | Shape3Extensible | Shape3Mutable | Shape3MutableExplicitID
Shape4Default | Shape4Final | Shape4Extensible | Shape4Mutable | Shape4MutableExplicitID
Shape5Default | Shape5Final | Shape5Extensible | Shape5Mutable | Shape5MutableExplicitID

There are two executables. The first is called *Shapes_publisher* and can be configured via command-line parameters to publish  one of the 25 types. The second is called *Shapes_subscriber* and can be configured via command-line parameters to subscribe one one of the 25 types.

Regardless of the data-type the **DDS Topic** name is always the same and is called *XTYPESTestTopic*. An additional command-line parameter is used to specify the **DDS domain_id**.

By providing different commmand-line paramaters to the *Shapes_publisher* and *Shapes_subscriber* it is possible to test 625 combinations (25 squared) of the type being published combined with the type being subscribed.

In accordance with the **OMG DDS-XTYPES** specification some of these type combinations are expected to be interoperable whereas others are not. The specification of what should be interoperable along with a brief justification is provided in the *compatibility_matrix.xlsx* spreadsheet.

The following is an example execution:

ON THE SUBSCRIBER COMPUTER:
```
dds-xtypes$ ./rti_connext_dds_5.2_linux -sub -domain 0 -type Shape2Extensible 
Usage:  ./rti_connext_dds_5.2_linux [-pub | -sub] [-domain <domainId>] [-type <typeName>]
Info: Starting subscribing application. Domain: 0, Type: Shape2Extensible

Waiting for data on topic "XTYPESTestTopic", type "Shape2Extensible"
on_subscription_matched: topic "XTYPESTestTopic", type "Shape2Extensible", count: 1, change: 1

Read data for Topic XTYPESTestTopic
   color: "BLUE"
   x: 1
   y: 2
   shapesize: 30
   angle: 0.000000

Read data for Topic XTYPESTestTopic
   color: "BLUE"
   x: 2
   y: 4
   shapesize: 30
   angle: 0.000000
....
```
ON THE PUBLISHER COMPUTER:
```
dds-xtypes$  ./rti_connext_dds_5.2_linux -pub -domain 0 -type Shape1Extensible
Usage:  ./rti_connext_dds_5.2_linux [-pub | -sub] [-domain <domainId>] [-type <typeName>]
Info: Starting publishing application. Domain: 0, Type: Shape1Extensible

Writing Topic "XTYPESTestTopic", type "Shape1Extensible", count 0, data:
   color: "BLUE"
   x: 0
   y: 0
   shapesize: 30
Writing Topic "XTYPESTestTopic", type "Shape1Extensible", count 1, data:
   color: "BLUE"
   x: 1
   y: 2
   shapesize: 30
Writing Topic "XTYPESTestTopic", type "Shape1Extensible", count 2, data:
   color: "BLUE"
   x: 2
   y: 4
   shapesize: 30
....
```

