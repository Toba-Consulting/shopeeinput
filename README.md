# Shopee Input — Hop transform plugin

A Hop pipeline transform that pulls product and order data from the Shopee Open Platform and
introduces it into a pipeline.

The plugin jar is already built and bundled in this folder:

```
hop-transform-shopeeinput-1.0.0-SNAPSHOT.jar
```

> **Built for Apache Hop 2.20.0.** Use a matching Hop client version, otherwise it may fail to load.

---

## Installation

1. Find your Hop client's `plugins` folder. It sits next to the `hop-gui.bat` you start Hop with:

   ```
   <your-hop>\plugins\transforms\shopeeinput\
   ```

2. Create the `shopeeinput` folder if it does not exist.

3. Copy the jar into it:

   ```
   hop-transform-shopeeinput-1.0.0-SNAPSHOT.jar
   ->
   <your-hop>\plugins\transforms\shopeeinput\hop-transform-shopeeinput-1.0.0-SNAPSHOT.jar
   ```

4. Create a file `version.xml` next to the jar, containing this one line:

   ```xml
   <version>1.0.0-SNAPSHOT</version>
   ```

5. Restart HOP GUI

The transform **"Shopee input"** now appears under the **Input** category.

