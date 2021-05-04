# Dataweave Scripts

- DW is the **primary data transformation language** for use in Mule flows. 

- DW 2.0 is for Mule 4 apps. For a Mule 3 app, refer to the DW 1.0 documentation. For other Mule versions, you can use the version selector for the Mule Runtime table of contents.

- You can write standalone DW scripts in:
    - Transform Message components
    - Configuration fields in an event processor or global configuration element, with inline expressions.
        - Inline DW expressions are enclosed in `#[ ]` code blocks. Example:
            ```
            <set-variable value="#[now()]" variableName="timestamp" doc:name="Set timestamp" />
            ```
- You can also store DW code in external files (`.dwl`), and read them into other DW scripts. 
    - Thus, you can **factor** DW code into modules (libraries) of reusable functions that can be shared by all the components in a Mule app.

## [The Structure of DataWeave Scripts](../language-guide.md#dw-script)

## DataWeave Header

- Example:

```
    %dw 2.0                        # %dw: DW version, optional. Default: 2.0
    import * from dw::core::Arrays # import: Import a DW function module.
    
    var myVar=13.15                # var: Global variables you can reference 
                                   # throughout the script (just using myVar).
    
    fun toUser(obj) = {            # Declares custom functions that can be called 
        firstName: obj.field1,     # from within the body of the script (just using toUser(payload)).
        lastName: obj.field2
    }
    
    type Currency                  # type: Specifies a custom type that you can use in the script
        = String { format: “##“}

    ns ns0 http://www.abc.com      # ns: Namespaces, used to import a namespace
                                   # (just using ns0#someroot: <value>).

    output application/xml         # output: Directive that specifies the mime type 
    ---                            # that the script outputs. 
    /*                             # Default: explained [here](#default-output-explanation) 
     * Body here.
     * /

```

### Default Output Explanation

- If no output is specified, the default output is determined by an algorithm that examines the inputs (payload, variables, and so on) used in the script:

    1. If there is no input, the default is `output application/java`.

    2. If all inputs are the same mime type, the script outputs to the same mime type. For example, if all input is application/json, then it outputs `output application/json`.

    3. If the mime types of the inputs differ, and no output is specified, the script throws an exception so that you know to specify an output mime type.

    Note that only one output type can be specified.

### Including Headers in Inline DataWeave Scripts

- You can include it by flattening all the lines in the (small) DW script into a single line. Example:
    ```
    <set-payload value="#[output application/xml --- { myroot: payload } ]" doc:name="Set Payload" />
    ```

