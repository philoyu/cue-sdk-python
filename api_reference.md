# API Reference

    
## Class `CueSdk`


```python
class CueSdk(sdk_path=None)
```
    
## Methods
    
### Method `connect`

    
```python
def connect(self)
```
    
### Method `get_device_count`

    
```python
def get_device_count(self)
```

Returns number of connected Corsair devices.

For keyboards, mice, mousemats, headsets and headset stands not more
than one device of each type is included in return value in case if
there are multiple devices of same type connected to the system.
For DIY-devices and coolers actual number of connected devices is
included in return value. For memory modules actual number of
connected modules is included in return value, modules are enumerated
with respect to their logical position (counting from left to right,
from top to bottom).

Use [get_device_info()](#method-get_device_info "get_device_info") to get information about a certain device.


    
### Method `get_device_info`


    
```python
def get_device_info(self, device_index)
```

Returns information about a device based on provided index.


##### Args

**`device_index`**
:   zero-based index of device. Should be strictly
    less than a value returned by [get_device_count()](#method-get_device_count "get_device_count")

    
### Method `get_devices`


    
```python
def get_devices(self)
```

    
### Method `get_last_error`


    
```python
def get_last_error(self)
```

Returns last error that occurred in this thread while using
any of SDK functions.

    
### Method `get_led_colors_by_device_index`


    
```python
def get_led_colors_by_device_index(self, device_index, led_identifiers)
```

Gets current color for the requested LEDs.

The color should represent the actual state of the hardware LED, which
could be a combination of SDK and/or CUE input. This function works
for keyboard, mouse, mousemat, headset, headset stand, DIY-devices,
memory module and cooler.


##### Args

**`device_index`**
:   zero-based index of device. Should be strictly less
    than value returned by [get_device_count()](#method-get_device_count "get_device_count")


**`led_identifiers`**
:   the list of `CorsairLedId` values



##### Returns

A dict mapping `CorsairLedId` keys to the corresponding color
values. Each color is represented as a tuple of ints. For
example:

```python
{
  CorsairLedId.K_Escape: (255, 0, 0),
  CorsairLedId.K_F1: (128, 0, 128)
}
```

If error occurred, the function returns None


    
### Method `get_led_id_for_key_name`


    
```python
def get_led_id_for_key_name(self, key_name)
```

Retrieves led id for key name taking logical layout into account.

So on AZERTY keyboards if user calls `get_led_id_for_key_name('A')` he
gets CorsairLedId.K_Q. This id can be used in
[set_led_colors_buffer_by_device_index()](#method-set_led_colors_buffer_by_device_index "set_led_colors_buffer_by_device_index") function


##### Args

**`key_name`**
:   key name. ['A'..'Z'] (26 values) are valid values



##### Returns

Proper `CorsairLedId` or CorsairLedId.Invalid if error
occurred


    
### Method `get_led_positions_by_device_index`


    
```python
def get_led_positions_by_device_index(self, device_index)
```

Provides dictionary of keyboard, mouse, headset, mousemat,
headset stand, DIY-devices, memory module and cooler LEDs by its index
with their positions.

Position could be either physical (only device-dependent)
or logical (depend on device as well as CUE settings).


##### Args

**`device_index`**
:   zero-based index of device. Should be strictly less
    than value returned by [get_device_count()](#method-get_device_count "get_device_count")



##### Returns

A dict mapping `CorsairLedId` keys to the corresponding LED
positions. Each position is represented as a tuple of floats.
For example:

```python
{ CorsairLedId.K_Escape: (77.0, 36.0) }
```

If error occurred, the function returns None


    
### Method `get_property`


    
```python
def get_property(self, device_index, property_id)
```


    
### Method `release_control`


    
```python
def release_control(self)
```

Releases previously requested exclusive control.


    
### Method `request_control`


    
```python
def request_control(self)
```

Requests exclusive control over lighting.

By default client has shared control over lighting so there is
no need to call [request_control()](#method-request_control "request_control") unless a client
requires exclusive control.


    
### Method `set_layer_priority`


    
```python
def set_layer_priority(self, priority)
```

Sets layer priority for this shared client.

By default CUE has priority of 127 and all shared clients have
priority of 128 if they don't call this function. Layers with
higher priority value are shown on top of layers with lower priority.


##### Args

**`priority`**
:   priority of a layer [0..255]



##### Returns

boolean value. True if successful. Use [get_last_error()](#method-get_last_error "get_last_error")
to check the reason of failure. If this function is called in
exclusive mode then it will return True


    
### Method `set_led_colors_buffer_by_device_index`


    
```python
def set_led_colors_buffer_by_device_index(self, device_index, led_colors)
```

Sets specified LEDs to some colors.

This function set LEDs colors in the buffer which is written to the
devices via [set_led_colors_flush_buffer()](#method-set_led_colors_flush_buffer "set_led_colors_flush_buffer") or
[set_led_colors_flush_buffer_async()](#method-set_led_colors_flush_buffer_async "set_led_colors_flush_buffer_async").
Typical usecase is next: [set_led_colors_flush_buffer()](#method-set_led_colors_flush_buffer "set_led_colors_flush_buffer") or
[set_led_colors_flush_buffer_async()](#method-set_led_colors_flush_buffer_async "set_led_colors_flush_buffer_async") is called to write LEDs
colors to the device and follows after one or more calls of
[set_led_colors_buffer_by_device_index()](#method-set_led_colors_buffer_by_device_index "set_led_colors_buffer_by_device_index") to set the LEDs buffer.
This function does not take logical layout into account.


##### Args

**`device_index`**
:   zero-based index of device. Should be strictly less
    than value returned by [get_device_count()](#method-get_device_count "get_device_count")


**`led_colors`**
:   a dict mapping `CorsairLedId` keys to the corresponding
    color values. Each color is represented as a tuple of ints
    (r, g, b). For example:



```python
{ CorsairLedId.K_F1: (255, 0, 0) }
```


    
### Method `set_led_colors_flush_buffer`


    
```python
def set_led_colors_flush_buffer(self)
```

Writes to the devices LEDs colors buffer which is previously filled
by the [set_led_colors_buffer_by_device_index()](#method-set_led_colors_buffer_by_device_index "set_led_colors_buffer_by_device_index") function.

This function executes synchronously, if you are concerned about
delays consider using [set_led_colors_flush_buffer_async()](#method-set_led_colors_flush_buffer_async "set_led_colors_flush_buffer_async")


    
### Method `set_led_colors_flush_buffer_async`


    
```python
def set_led_colors_flush_buffer_async(self)
```

Same as [set_led_colors_flush_buffer()](#method-set_led_colors_flush_buffer "set_led_colors_flush_buffer") but returns control to
the caller immediately


    
### Method `subscribe_for_events`


    
```python
def subscribe_for_events(self, handler)
```

Registers a callback that will be called by SDK when some event
happened.

If client is already subscribed but calls this function again SDK
should use only last callback registered for sending notifications.


##### Args

handler:
    Callback function with two arguments: event_id and event_data

##### Returns

boolean value. True if successful. Use [get_last_error()](#method-get_last_error "get_last_error")
to check the reason of failure.


    
### Method `unsubscribe_from_events`


    
```python
def unsubscribe_from_events(self)
```

Unregisters callback previously registered by
[subscribe_for_events()](#method-subscribe_for_events "subscribe_for_events") call


##### Returns

boolean value. True if successful. Use [get_last_error()](#method-get_last_error "get_last_error")
to check the reason of failure.
