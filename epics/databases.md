# EPICS Databases Concepts

An EPICS-based control system contains one or more Input Output Controllers, IOCs. Each IOC loads one or more databases.
A database is a collection of records of various types.

A Record is an object with:
* A unique name
* A behavior defined by its type
* Controllable properties (fields)
* Optional associated hardware I/O (device support)
* Links to other records

There are several different types of records available. In addition to the record types that are included in the EPICS 
base software package, it is possible (although not recommended unless you absolutely need) to create your own record 
type to perform some specific tasks.

Each record comprises a number of fields. Fields can have different functions, typically they are used to configure how 
the record operates, or to store data items.

An EPICS record is defined as follows:
```
record(RECORD_TYPE, RECORD_NAME)
{
    field(FIELDNAME, "FIELDVALUE")
    field(FIELDNAME, "FIELDVALUE")
    .
    .
}
```

## Database Functionality Specification
This chapter covers the general functionality that is found in all database records. The topics covered are I/O scanning, 
I/O address specification, data conversions, alarms, database monitoring, and continuous control:
* Scanning Specification describes the various conditions under which a record is processed.
* Address Specification explains the source of inputs and the destination of outputs.
* Conversion Specification covers data conversions from transducer interfaces to engineering units.
* Alarm Specification presents the many alarm detection mechanisms available in the database.
* Monitor Specification details the mechanism, which notifies operators about database value changes.
* Control Specification explains the features available for achieving continuous control in the database.

These concepts are essential in order to understand how the database interfaces with the process.

## Scanning Specification
Scanning determines when a record is processed. A record is processed when it performs any actions related to its data. 
For example, when an output record is processed, it fetches the value which it is to output, converts the value, and then 
writes that value to the specified location. Each record must specify the scanning method that determines when it will be 
processed. There are three scanning methods for database records:
* Periodic scanning occurs on set time intervals.
* Event scanning occurs on either an I/O interrupt event or a user-defined event.
* Passive scanning occurs when the records linked to the passive record are scanned, or when a value is “put” into a passive
  record through the database access routines.

For periodic or event scanning, the user can also control the order in which a set of records is processed by using the PHASE
mechanism. The number in the PHAS field allows to define the relative order in which records are processed within a scan cycle:
* Records with PHAS=0 are processed first
* Then those with PHAS=1, PHAS=2, etc.

For event scanning, the user can control the priority at which a record will process. The PRIO field selects Low/Medium/High 
priority for Soft event and I/O Interrupts. In addition to the scan and the phase mechanisms, there are data links and forward 
processing links that can be used to cause processing in other records.

### Periodic Scanning
The periodic scan tasks run as close as possible to the specified frequency. When each periodic scan task starts, it calls the 
gettime routine, then processes all of the records on this period. After the processing, gettime is called again and this thread 
sleeps the difference between the scan period and the time to process the records. For example, if it takes 100 milliseconds to 
process all records with “1 second” scan period, then the 1 second scan period will start again 900 milliseconds after completion. 
The following periods for scanning database records are available by default, though EPICS can be configured to recognize more
scan periods:
* 10 second
* 5 second
* 2 second
* 1 second
* .5 second
* .2 second
* .1 second

The period that best fits the nature of the signal should be specified. A five-second interval is adequate for the temperature of 
a mass of water because it does not change rapidly. However, some power levels may change very rapidly, so they need to be scanned 
every 0.5 seconds. In the case of a continuous control loop, where the process variable being controlled can change quickly, the 0.1 
second interval may be the best choice.

### Passive Scanning
Passive records are processed when they are referenced by other records through their link fields or when a channel access put is 
done to them.
* Channel Access Puts: In this case where a channel access put is done to a record, the field being written has an attribute that
  determines if this put causes record processing. In the case of all records, putting to the VAL field causes record processing.
* Database Links: The records in the process database use link fields to configure data passing and scheduling (or processing).
  These fields are either INLINK, OUTLINK, or FWDLINK fields.

### Forward Links
Forward Links are defined using the `FLNK` field. If the record that is referenced by the FLNK field has a SCAN field set to 
`Passive`, then the record is processed after the record with the FLNK. The FLNK field only causes record processing, no data is 
passed.

Input links normally fetch data from one field into a field in the referring record. For instance, if the INPA field of a CALC 
record is set to Input_3.VAL, then the VAL field is fetched from the Input_3 record and placed in the A field of the CALC record. 
These data links have an attribute that specify if a passive record should be processed before the value is returned. The default 
for this attribute is NPP (no process passive). In this case, the record takes the VAL field and returns it. If they are set to 
PP (process passive), then the record is processed before the field is returned.

### Channel Access Links
A Channel Access link is an input link or output link that specifies a link to a record located in another IOC or an input and 
output link with one of the following attributes: CA, CP, or CPP.

If the input link specifies CA, CP, or CPP, regardless of the location of the process variable being referenced, it will be forced 
to be a Channel Access link. This is helpful for separating process chains that are not tightly related. If the input link specifies 
CP, it also causes the record containing the input link to process whenever a monitor is posted, no matter what the record’s SCAN 
field specifies. If the input link specifies CPP, it causes the record to be processed if and only if the record with the CPP link 
has a SCAN field set to Passive. In other words, CP and CPP cause the record containing the link to be processed with the process 
variable that they reference changes.

Only CA is appropriate for an output link. The write to a field over channel access causes processing as specified in Channel Access 
Puts to Passive Scanned Records.

Forward links can also be Channel Access links, either when they specify a record located in another IOC or when they specify the CA 
attributes. However, forward links will only be made Channel Access links if they specify the PROC field of another record.

## Address Specifications
Address parameters specify where an input record obtains input, where an output record obtains its desired output values, and where 
an output record writes its output. They are used to identify links between records, and to specify the location of hardware devices. 
The most common link fields are OUT, an output link, INP, an input link, and DOL (desired output location), also an input link.

There are three basic types of address specifications, which can appear in these fields: hardware addresses, database addresses, and 
constants. Note that not all links support all three types, though some do. However, this doesn’t hold true for algorithmic records, 
which cannot specify hardware addresses. Algorithm records are records like the Calculation, PID, and Select records. These records 
are used to process values retrieved from other records

### Hardware Addresses
The interface between EPICS process database logic and hardware drivers is indicated in two fields of records that support hardware 
interfaces: DTYP and INP/OUT. The DTYP field is the name of the device support entry table that is used to interface to the device. 
The address specification is dictated by the device support. Some conventions exist for several buses that are listed below. Lately,
more devices have just opted to use a string that is then parsed by the device support as desired. This specification type is called 
INST I/O. The other conventions listed here include: VME, Allen-Bradley, CAMAC, GPIB, BITBUS, VXI, and RF.

#### INST_IO
The INST I/O specification is a string that is parsed by the device support. The format of this string is determined by the device 
support. For INST I/O, an `@` precedes optional string parm.

### Database Addresses
Database addresses are used to specify input links, desired output links, output links, and forward processing links. The format in 
each case is the same:
```
<RecordName>.<FieldName>
```
where RecordName is simply the name of the record being referenced, ‘.’ is the separator between the record name and the field name, 
and FieldName is the name of the field within the record. The record name and field name specification are case sensitive. The record
name can be a mix of the following: a-z A-Z 0-9 _ - : . [ ] < > ;. The field name is always upper case. If no field name is specified 
as part of an address, the value field (VAL) of the record is assumed. Forward processing links do not need to include the field name
because no value is returned when a forward processing link is used; therefore, a forward processing link need only specify a record 
name.
