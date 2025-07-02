# EPICS Records Reference
Here we will briefly describe many types of record types, menus, link types and channel filters included with EPICS Base. Many details about the record and menu definitions are derived automatically from the source code at build time.

## Fields Common to All Record Types

### Operator Display Parameters
*   **NAME**: Contains the **unique record name** within an EPICS Channel Access name space. It can be up to 60 characters long and uses a limited set of characters (a-z, A-Z, 0-9, _, -, :, [, ], <, >, ;).
*   **DESC**: Provides a **meaningful description** of the record's purpose, with a maximum length of 40 characters.

### Scan Fields
These fields manage how and when a record processes.
*   **SCAN**: Specifies the **scanning period for periodic record scans** or the **scan type for non-periodic record scans**. Choices include:
    *   **Passive**: Triggered by other records or Channel Access Events.
    *   **I/O Intr**: Interrupt-driven scan.
    *   A set of **periodic scan intervals** (e.g., "1 hour", "3 seconds", "2 Hertz").
*   **PINI**: Specifies **record processing at initialization** (IOC initialization). Values include NO, YES, RUN, RUNNING, PAUSE, and PAUSED. If set to YES, the record processes once at IOC initialization.
*   **PHAS**: **Orders records processed within a specific SCAN group or PINI processing phase**. Records with a lower phase number are processed before those with higher numbers.
*   **EVNT**: Specifies an **event name or number** used if the SCAN field is set to "Event". Records with the same event identifier and "Event" scan type are processed when that event is signaled.
*   **PRIO**: Specifies the **scheduling priority** for processing records with `SCAN=I/O Event` and asynchronous record completion tasks.
*   **DISV**: Defines a "**disable value**". Record processing is prevented when this field's value equals the **DISA** field's value.
*   **DISA**: Contains the **value compared with DISV** to determine if the record is disabled.
*   **SDIS**: A link field from which the DISA field obtains its value before record processing.
*   **PROC**: If written to, **forces the record to be processed**.
*   **DISS**: Defines the record's "**disable severity**". If not `NO_ALARM` and the record is disabled, it will be put into alarm with this severity and `DISABLE_ALARM` status.
*   **LSET**: Contains the **lock set** to which the record belongs. All linked records (via input, output, or forward database links) belong to the same lock set.
*   **LCNT**: Counts the number of times `dbProcess` finds the record active during successive scans. If it finds the record active `MAX_LOCK` times (currently 10), it raises a `SCAN_ALARM`.
*   **PACT**: Is **TRUE while the record is active** (being processed). When PACT is TRUE, `dbProcess` will not call the record processing routine.
*   **FLNK**: A **link pointing to another record** (the "target" record). Processing a record with FLNK set will trigger processing of the target record, provided the target record's SCAN field is set to "Passive".
*   **SPVT**: For **internal use by the scanning system**.

### Alarm Fields
These fields indicate alarm status and severity, or determine how alarms are triggered.
*   **STAT**: Contains the **current alarm status**.
*   **SEVR**: Contains the **current alarm severity**.
*   **AMSG**: A string field that may contain **more detailed information about the alarm**.
*   **NSTA, NSEV, NAMSG**: Used during record processing to **set new alarm status, severity, and message text**. These fields always relate to the highest severity alarm seen during processing.
*   **ACKS**: Contains the **highest unacknowledged alarm severity**.
*   **ACKT**: Specifies whether it is **necessary to acknowledge transient alarms**.
*   **UDF**: Indicates if the **record's value is UnDeFined**. This can be due to device support failure, the record never being processed, or the VAL field containing `NaN` or `Inf`. UDF defaults to TRUE.
*   **UDFS**: Specifies the **alarm severity** the record will be set to when its value is **undefined** (i.e., UDF is 1).

### Device Fields
*   **RSET**: Contains the **address of the Record Support Entry Table**.
*   **DSET**: Contains the **address of Device Support Entry Table**. Record support routines use this field to locate their device support routines.
*   **DPVT**: For **private use of the device support modules**.

### Debugging Fields
*   **TPRO**: Used to **trace record processing**. When non-zero, a trace message is printed for this record and any other record in the same lock-set triggered by a database link from it.
*   **BKPT**: Indicates if there is a **breakpoint set at this record**, supporting debug breakpoints and step-through processing.

### Miscellaneous Fields
*   **ASG**: Sets the **name of the access security group** for the record. If empty, the record is placed in group `DEFAULT`.
*   **ASP**: Private for use by the **access security system**.
*   **DISP**: If non-zero, **rejects puts from outside the IOC** (e.g., via Channel Access or PV Access) to any field of the record, except the DISP field itself.
*   **DTYP**: Specifies the **device type for the record**.
*   **MLOK**: Contains a mutex locked by **monitor routines** when the monitor list for this record is accessed.
*   **MLIS**: Holds a **linked list of client monitors** connected to this record.
*   **PPN**: Contains the **address of a putNotify callback**.
*   **PPNR**: Contains the **next record for PutNotify**.
*   **PUTF**: Set to TRUE if `dbPutField` caused the **current record processing**.
*   **RDES**: Contains the **address of `dbRecordType`**.
*   **RPRO**: Specifies a **reprocessing of the record** when current processing completes.
*   **TIME**: Holds the **time stamp** when this record was last processed.
*   **UTAG**: Can hold a **site-specific 64-bit User Tag value** associated with the record's time stamp.
*   **TSE**: Indicates the **mechanism to use to get the time stamp**:
    *   `0`: Get the current time as normal.
    *   `-1`: Ask the time stamp driver for its best source.
    *   `-2`: Device support sets the time stamp and optional User Tag from hardware.
    *   Positive values (1-255): Get the time of the last occurrence of the numbered `generalTime` event.
*   **TSEL**: An **input link for obtaining the time stamp**. If it points to a record's TIME field, that record's time stamp and User Tag are copied directly. If it points to any other field, that field's value is read and stored in TSE.

## Fields Common to Input Records
This section focuses on fields found in many EPICS input record types that generally have consistent meanings.

*   **Input and Value Fields**
    *   **INP**: Specifies an **input link** used by device support routines to obtain input. For soft analog records, it can be a constant, a database link, or a channel access link.
    *   **DTYP**: Specifies the name of the **device support module** responsible for inputting values. Each record type has its own set of device support routines, and `DTYP` is meaningless if a record type lacks associated device support.
    *   **RVAL**: Contains the **raw data value** directly from hardware or the device driver, before any conversions. The Soft Channel device support module bypasses this field, reading values directly into `VAL`.
    *   **VAL**: Contains the record's **final value** after any necessary conversions have been performed.

*   **Device Input**
    *   A device input routine typically returns one of two values to its associated record support routine:
        *   **0**: Indicates **success and conversion**. The input value is placed in `RVAL`, and the record support module computes `VAL` from `RVAL`.
        *   **2**: Indicates **success but no conversion**. The device support module specifies this if it doesn't want conversions, possibly due to a hardware error (in which case an alarm condition should also be raised) or if it reads values directly into the `VAL` field and sets `UDF` to FALSE.
    *   The device support read routine usually calls `dbGetLink()` to fetch a value from the link. If a value is returned, the `UDF` field is set to `FALSE`.

*   **Device Support for Soft Records**
    *   Two common soft output device support modules are **Soft Channel** and **Raw Soft Channel**.
    *   Both allow `INP` to be a constant, database link, or channel access link.
    *   **Soft Channel** reads input **directly into the `VAL` field** and specifies no value conversion, allowing the record to store values in the `VAL` field's data type. For this input, the `RVAL` field is **not used**.
    *   **Raw Soft Channel** reads input into **`RVAL`** and indicates that any specified unit conversions should be performed.

*   **Input Simulation Fields**
    *   **SIMM**: Controls **simulation mode**. Setting it to `YES` or `RAW` switches the record into simulation mode, where input is obtained from `SIOL` instead of `INP`. If `SIML` is a database or channel access link, `SIMM` is read from `SIML`. If `SIML` is a constant link, `SIMM` is initialized with that constant value but can be changed.
    *   **SIML**: Specifies the **simulation mode location**, which can be a constant, a database link, or a channel access link.
    *   **SVAL**: Contains the **simulation value**, which is the record's input value in engineering units when `SIMM` is `YES` or `RAW`.
    *   **SIOL**: A link used to **fetch the simulation value (`SVAL`)**. It can be a constant, a database link, or a channel access link. If it's a link, `SVAL` is read from `SIOL`. If it's a constant, `SVAL` is initialized with that constant but can be changed.
    *   **SIMS**: Specifies the **simulation mode alarm severity**. If set to a value other than `NO_ALARM` and the record is in simulation mode, it will enter an alarm state with this severity and a status of `SIMM_ALARM`.
    *   **SDLY**: Specifies a **delay (in seconds)** for asynchronous processing in simulation mode. A positive value is a delay between processing phases; a negative value (default) indicates synchronous processing.
    *   **SSCN**: Specifies the **SCAN mechanism** to be used in simulation mode, particularly useful for 'I/O Intr' scanned records which would otherwise not be scanned.

*   **Simulation Mode for Input Records**
    *   An input record enters simulation mode by setting `SIMM` to `YES` or `RAW`.
    *   When in simulation mode, the record will enter an alarm state with severity `SIMS` and status `SIMM_ALARM`.
    *   **If `SIMM` is `YES`**, the input value (in engineering units) is obtained from `SIOL` and **directly written to `VAL`**.
    *   **If `SIMM` is `RAW`**, the value from `SIOL` is truncated and **written to `RVAL`**, followed by the regular raw value conversion.
    *   While in simulation mode, there are **no calls to device support** when the record is processed.
    *   If `SIOL` contains a link, a `TSE` (Time Stamp Event) setting of "time from device" (-2) is honored by taking the timestamp from the record `SIOL` points to.

## Fields Common to Output Records
This section describes fields found in many EPICS output record types, which generally share consistent meanings.

*   **Output and Value Fields**
    *   **OUT**: Specifies an **output link** used by device support routines to send output. For soft records, it can be a constant, a database link, or a channel access link. If it's a constant, no output is produced.
    *   **DTYP**: Specifies the name of the **device support module** responsible for inputting values. This field is meaningless if a record type lacks associated device support routines.
    *   **VAL**: Contains the **desired value** before any conversions to raw output have been performed.
    *   **OVAL**: Used to determine when to invoke monitors. **Archive and value change monitors are invoked if `OVAL` is not equal to `VAL`**. It also helps enforce a maximum rate of change limit before conversion when a record type needs adjustments.
    *   **RVAL**: Contains, whenever possible, the **actual value sent to the hardware** or associated device driver.
    *   **RBV**: Contains, whenever possible, the **actual read-back value** obtained from the hardware or device driver.

*   **Device Support for Soft Records**
    *   Two common soft output device support modules are **Soft Channel** and **Raw Soft Channel**.
    *   Both modules write a value through the `OUT` link.
    *   The **Soft Channel** module writes output from the value associated with `OVAL` or `VAL` (if `OVAL` doesn't exist).
    *   The **Raw Soft Channel** support module writes the value associated with the `RVAL` field after conversion has been performed.
    *   The device support write routine typically calls `dbPutLink()` to write a value via the `OUT` link and returns the status of that call.

*   **Input and Mode Select Fields & Output Mode Selection**
    *   **DOL**: A link from which the **desired output value can be fetched**. It can be a constant, a database link, or a channel access link. `VAL` is obtained from `DOL` if `DOL` is a database or channel access link and `OMSL` is `closed_loop`.
    *   **OMSL**: Selects the **output mode**, with values `supervisory` or `closed_loop`. `DOL` is used to fetch `VAL` only if `OMSL` is `closed_loop`. Setting `OMSL` allows a record to switch between supervisory and closed-loop operation.
    *   If `OMSL` is `closed_loop` and the record type does not contain an `OIF` field, `VAL` is set equal to the value from `DOL` each time the record is processed.
    *   If `OMSL` is `closed_loop` in record types with an `OIF` field:
        *   If `OIF` is `Full`, `VAL` is set equal to the value obtained from `DOL`.
        *   If `OIF` is `Incremental`, `VAL` is incremented by the value obtained from `DOL`.
    *   When in `closed_loop` mode, the `VAL` field cannot be set directly via `dbPuts`. `OMSL` is only meaningful if `DOL` refers to a database or channel access link.

*   **Invalid Output Action Fields**
    *   **IVOA**: Specifies the **action to take when the record is put into an `INVALID` alarm severity**. It can be:
        *   `Continue normally`
        *   `Don't drive outputs`
        *   `Set output to IVOV`
    *   **IVOV**: Contains the **value in engineering units** for the `IVOA` action `Set output to IVOV`. If a new severity is `INVALID` and `IVOA` is `Set output to IVOV`, then `VAL` is set to `IVOV` and converted to `RVAL` before device support is called.
    *   The record support process routine checks the new severity:
        *   If less than `INVALID`, `writeValue()` is called.
        *   If `INVALID`:
            *   If `IVOA` is `Continue normally`, `writeValue()` is called.
            *   If `IVOA` is `Don't drive outputs`, no output is written.
            *   If `IVOA` is `Set output to IVOV`, `VAL` is set to `IVOV`, converted if necessary, and then `writeValue()` is called.
            *   If `IVOA` is none of the above, an error message is generated.

*   **Output Simulation Fields & Simulation Mode for Output Records**
    *   **SIMM**: Controls **simulation mode**, with values `YES` or `NO`. Setting it to `YES` switches the record into simulation mode.
    *   **SIML**: Specifies the **simulation mode location**. It can be a constant, a database link, or a channel access link. If it's a link, `SIMM` is read from `SIML`; if it's a constant, `SIMM` is initialized with that constant but can be changed.
    *   **SIOL**: A link to which the **output value is written when the record is in simulation mode**.
    *   **SIMS**: Specifies the **simulation mode alarm severity**. If set to a value other than `NO_ALARM` and the record is in simulation mode, it will enter an alarm state with this severity and a status of `SIMM_ALARM`.
    *   **SDLY**: Specifies a **delay (in seconds)** for asynchronous processing in simulation mode. A positive value delays between processing phases; a negative value (default) indicates synchronous processing.
    *   **SSCN**: Specifies the **SCAN mechanism to be used in simulation mode**. This is particularly useful for 'I/O Intr' scanned records, which would otherwise not be scanned in simulation mode.
    *   While in simulation mode, output values (in engineering units) are written to `SIOL` instead of `OUT`. However, these output values are **never converted**. Also, **no calls to device support** occur during record processing.
