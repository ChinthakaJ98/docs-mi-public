# Loopback Mediator

The **Loopback Mediator** moves messages from the in flow (request path) to the out flow (response path). All the configuration included in the in sequence that appears after the Loopback mediator is skipped.

!!! Info
    The messages that have already been passed from the In sequence to the Out sequence cannot be moved to the Out sequence again via the Loopback mediator. 

## Syntax

The loopback token refers to a `<loopback/>` element, which is used to skip the rest of the in flow and move the message to the out flow.

``` java
<loopback/>
```

## Example

This example is a main sequence configuration with two [Payload mediators]({{base_path}}/reference/mediators/payloadfactory-mediator). Assume you only want to use the
first factory but need to keep the second factory in the configuration for future reference. The Loopback mediator is added after the first
Payload mediator configuration to skip the second Payload mediator configuration. This configuration will cause the message to be processed
with the first Payload mediator and then immediately move to the out flow, skipping the second Payload mediator in the in flow.

``` java
<proxy name="SimpleProxy" transports="http https" startOnLoad="true" trace="disable" xmlns="http://ws.apache.org/ns/synapse">
    <target>
      <inSequence>
      <payloadFactory media-type="xml">
        <format>
          <m:messageBeforeLoopBack xmlns:m="http://services.samples">
            <m:messageBeforeLoopBackSymbol>
              <m:symbolBeforeLoopBack>$1</m:symbolBeforeLoopBack>
            </m:messageBeforeLoopBackSymbol>
          </m:messageBeforeLoopBack>
        </format>
        <args>
          <arg xmlns:m0="http://services.samples"
            evaluator="xml"
            expression="//m0:symbol/text()"/>
          </args>
      </payloadFactory>

      <loopback/>

      <payloadFactory media-type="xml">
        <format>
          <m:messageAfterLoopBack xmlns:m="http://services.samples">
            <m:messageAfterLoopBackSymbol>
              <m:symbolAfterLoopBack>$1</m:symbolAfterLoopBack>
            </m:messageAfterLoopBackSymbol>
          </m:messageAfterLoopBack>
        </format>
        <args>
          <arg xmlns:m0="http://services.samples"
            evaluator="xml"
            expression="//m0:symbolBeforeLoopBack/text()"/>
        </args>
      </payloadFactory>
    </inSequence>
    <outSequence>
      <send/>
    </outSequence>
  </target>
</proxy>
```
