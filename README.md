Consistent Overhead Byte Stuffing Transform Streams
===

Encode or decode using COBS (Consistent Overhead Byte Stuffing) using
COBS Transfors Streams (https://streams.spec.whatwg.org/#ts-class)

More info: https://en.wikipedia.org/wiki/Consistent_Overhead_Byte_Stuffing

Example
--

Instantiation and use

```js
  const decoder = new TransformStream(new COBSDecoderTransformer);
```

Reading data

```js
  const reader = decoder.readable.getReader();

  function processData(chunk) {
    let { done, value } = chunk;
    if (done) {
      return;
    }

    const view = new DataView(value.buffer);

    // do stuff with data.
    return reader.read().then(processData);
  };

  reader.read().then(processData);
```

Writing data

```js
  const writer = decoder.readable.getWriter();

  async readFromDevice() {
    // Example using Web USB to get 32 bytes on endpoint 5
    const transfer = await this.device.transferIn(5, 32);

    const data = new Uint8Array(transfer.data.buffer);
    writer.write(data);

    this.readFromDevice();
  }
```

