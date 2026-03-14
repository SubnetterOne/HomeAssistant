# Custom Wake Words for ESPHome Assist Devices

This folder contains custom wake words that can be used with **ESPHome Assist devices**, such as the **M5Stack Atom Echo**.

If you want more background on this repository and my Home Assistant setup, please start with the **main repository README**.

---

# What This Folder Contains

This directory holds everything needed to add custom wake words.

Example structure:

```
custom-wake-words
│
├── README.md
│
├── atom-echo-template.yaml
│
├── hey_atom
│   ├── hey_atom.json
│   └── hey_atom.tflite
│
└── hey_consuelo
    ├── hey_consuelo.json
    └── hey_consuelo.tflite
```

The primary example used in this guide is **hey_atom**.

There is also a **hey_consuelo** example included to demonstrate how you can keep adding additional wake words to your system.

Once you understand the structure, you can easily add more wake words later.

---

# Files Explained

## atom-echo-template.yaml

A working ESPHome template for **Atom Echo devices**.

You can copy this template and rename it for each device.

Examples:

```
atom-echo-001.yaml
atom-echo-office.yaml
atom-echo-kitchen.yaml
```

---

## hey_atom.tflite

This is the **trained wake word model**.

It is the machine learning model that listens for the wake phrase.

Example wake phrase:

```
Hey Atom
```

---

## hey_atom.json

This is the **manifest file**.

It tells ESPHome:

- the name of the wake word
- where the model file is
- tuning parameters for detection

---

# Folder Layout (IMPORTANT)

ESPHome expects this structure inside your Home Assistant system:

```
/config/esphome/
│
├── atom-echo-001.yaml
│
└── models
    ├── hey_atom.json
    └── hey_atom.tflite
```

If the files are not in the correct location, ESPHome will fail validation.

---

# Step-by-Step Setup

## Step 1 — Copy the Wake Word Files

Copy the wake word files into your ESPHome **models** folder.

Example:

```
/config/esphome/models/hey_atom.json
/config/esphome/models/hey_atom.tflite
```

---

## Step 2 — Create Your Device YAML

Copy the template file:

```
atom-echo-template.yaml
```

Rename it for your device.

Example:

```
atom-echo-001.yaml
```

Place it here:

```
/config/esphome/
```

---

## Step 3 — Add the Wake Word to ESPHome

Inside your device YAML, add the wake word configuration.

```
micro_wake_word:
  on_wake_word_detected:
    - voice_assistant.start:
        wake_word: !lambda return wake_word;

  vad:

  models:
    - model: models/hey_atom.json
```

Important:

Do **NOT** use a URL.

Use the local path:

```
models/hey_atom.json
```

---

# Example Working Configuration

```
micro_wake_word:
  on_wake_word_detected:
    - voice_assistant.start:
        wake_word: !lambda return wake_word;

  vad:

  models:
    - model: models/hey_atom.json
```

---

# Wake Word Sensitivity

Inside the JSON file you may see something like:

```
"probability_cutoff": 0.9
```

This controls how sensitive the wake word is.

Higher values:

• fewer false triggers  
• but harder to activate  

Lower values:

• easier to activate  
• but may trigger accidentally  

Typical values:

```
0.90 – 0.97
```

---

# Troubleshooting

### Error: Model Not Found

Check that these files exist:

```
/config/esphome/models/hey_atom.json
/config/esphome/models/hey_atom.tflite
```

---

### Error: Invalid Manifest File

Make sure the JSON file contains:

```
"model": "hey_atom.tflite"
```

Not a URL.

---

### Error: Not a Valid Model Name

Make sure your YAML contains:

```
models:
  - model: models/hey_atom.json
```

Not:

```
models:
  - model: hey_atom.json
```

---

# Adding More Wake Words

This directory already contains another example:

```
hey_consuelo
```

This is included to show how easy it is to keep adding more wake words to your system.

Once you understand the folder layout and configuration, you can create as many wake words as you want.

---

# Creating Your Own Wake Word

You can train custom wake words using tools such as:

OpenWakeWord  
https://github.com/dscripka/openWakeWord

ESPHome Micro Wake Word models  
https://github.com/esphome/micro-wake-word-models

More details are available in the **main repository README**.

---

# Final Notes

Once configured, your ESPHome Assist device will listen for the wake phrase defined in the model.

Example:

```
Hey Atom
```

When the phrase is detected, the device will start the Home Assistant voice assistant.

---

If this guide saves someone else from spending hours debugging wake word configs, then it did its job.