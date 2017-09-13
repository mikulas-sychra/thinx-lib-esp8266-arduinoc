# THiNX Lib (ESP)

An Arduino/ESP8266 library to wrap client for OTA updates and RTM [Remote Things Management](https://rtm.thinx.cloud) based on [THiNX OpenSource IoT platform](https://thinx.cloud).

# Success log (library is not finished)

```
* TH: Init with AK...
*TH: No remote configuration found so far...
*TH: Checking AK...
*TH: Exiting (no API Key)...
*TH: Starting loop...
.
Free size: 3222364160


Soft WDT reset

```

Stack trace:
```
Exception 9: LoadStoreAlignmentCause: Load or store to an unaligned address
Decoding 10 results
0x40201646: Print::write(char const*) at ?? line ?
0x402098e4: PubSubClient::loop() at ?? line ?
0x402017b5: Print::println(char const*) at ?? line ?
0x40204eb8: ArduinoJson::Internals::JsonParser  ::Reader, ArduinoJson::StaticJsonBufferBase&>::parseObject() at ?? line ?
0x4021415c: HTTPClient::~HTTPClient() at ?? line ?
0x4020164d: Print::write(char const*) at ?? line ?
0x4020164d: Print::write(char const*) at ?? line ?
0x40206229: THiNX::start_mqtt() at ?? line ?
0x40206400: THiNX::THiNX(char const*) at ?? line ?
0x40214e46: TransportTraits::verify(WiFiClient&, char const*) at ?? line ?

```
# Usage
## Include

```c
#include "THiNXLib.h"

```

## Definition
### THiNX Library

The singleton class started by library should not require any additional parameters except for optional API Key.
Connects to WiFI and reports to main THiNX server; otherwise starts WiFI in AP mode (AP-THiNX with password PASSWORD by default)
and awaits optionally new API Key (security hole? FIXME: In case the API Key is saved (and validated) do not allow change from AP mode!!!).

* if not defined, defaults to thinx.cloud platform
* TODO: either your local `thinx-device-api` instance or [currently non-existent at the time of this writing] `thinx-secure-gateway` which does not exist now, but is planned to provide HTTP to HTTPS bridging from local network to

```c
#include "Arduino.h"
#include <THiNXLib.h>

THiNX thx;

void setup() {
  Serial.begin(115200);

#ifdef __DEBUG__
  while (!Serial);
#else
  delay(500);
#endif

  thx = THiNX("71679ca646c63d234e957e37e4f4069bf4eed14afca4569a0c74abf503076732"); // THINX_API_KEY
}

void loop()
{
  delay(10000);
  thx.loop(); // registers, checks MQTT status, reconnects, updates, etc.
}

```
