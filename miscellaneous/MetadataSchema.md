---
layout: standard
title: YAML Schema for Plugin Metadata API
wrap_title: Metadata Schema
permalink: /docs/metadatarequirements/metadataschema/
---

~~~
--- # A metadata schema in Kwalify: http://www.kuwata-lab.com/kwalify/
# Version: 0.2
type: map
mapping:
  "schema_version": # This should match the version comment above
    type: float
    required: yes
  "LC_NUMERIC": # If not provided LC_NUMERIC=C is used, not the system's locale.
    type: str
    required: no
  "type": # A service type
    type: str
    required: yes
    enum: [consumer, filter, producer, transition]
  "identifier": # The same value used to register and create the service
    type: str
    required: yes
    unique: yes
  "title": # The UI can use this for a field label
    type: str
  "copyright": # Who owns the rights to the module and/or service?
    type: str
  "version": # The version of the service implementation
    type: text
  "license": # The software license for the service implementation
    type: str
  "language": # A 2 character ISO 639-1 language code
    type: str
    required: yes
  "url": # A hyperlink to a related website
    type: str
  "creator": # The name and/or e-mail address of the original author
    type: str
  "contributor": # The name and/or e-mail of all source code contributors
    type: seq
    sequence:
      - type: str
  "tags": # A set of categories, this might become an enum
    type: seq
    sequence:
      - type: str
  "description": # A slightly longer description than title
    type: str
  "icon": # A graphical representation of the effect
    type: map
    mapping:
      "filename":
        type: str
      "content-type":
        type: str
      "content-encoding":
        type: str
      "content":
        type: str
  "notes": # Details about the usage and/or implementation - can be long
    type: str
  "bugs": # A list of known problems that users can try to avoid
    type: seq
    sequence:
      - type: str # Can be a sentence or paragraph, preferably not a hyperlink
  "parameters": # A list of all of the options for the service
    type: seq
    sequence:
      - type: map
        mapping:
          "identifier": # The key that must be used to set the mlt_property
            type: str
            required: yes
          "type": # An mlt_property_type
            type: str
            enum:
              - boolean # 0 or 1; not 'true', 'false', 'yes', or 'no' strings at this time
              - float
              - geometry
              - integer
              - properties # for passing options to encapsulated services
              - string
              - time # time string values (clock, SMPTE) can be acccepted in addition to
                frames
          "service-name": # for type: properties, a reference to another service
            type: str     # format: type.service, e.g. transition.composite
          "title": # A UI can use this for a field label
            type: str
          "description": # A UI can use this for a tool tip or what's-this
            type: str
          "argument": # If this is also the service constructor argument.
            type: bool
            default: no
          "readonly": # If you set this property, it will be ignored
            type: bool
            default: no
          "required": # Is this property required?
            type: bool
            default: no
          "mutable": # The service will change behavior if this is set after
                     # processing the first frame
            type: bool
            default: no
          "widget": # A hint to the UI about how to let the user set this
            type: str
            enum:
              - checkbox
              - color
              - combo
              - curve
              - directory
              - fileopen
              - filesave
              - font
              - knob
              - listbox
              - dropdown # aka HTML select or GtkOptionMenu
              - radio
              - rectangle # for use with type: geometry
              - slider
              - spinner
              - text
              - textbox # multi-line
              - timecode
          "minimum": # For numeric types, the minimal value
            type: number
          "maximum": # For numeric types, the maximal value
            type: number
          "default":     # The default value to be used in a UI
            type: scalar # If not specified, the UI might be able to display
                         # this as blank
          "unit": # A UI can display this as a label after the widget (e.g. %)
            type: str
          "scale": # the number of digits after decimal point when type: float
            type: int
          "format": # A hint about a custom string encoding, possibly scanf
          "values": # A list of acceptable string values
            type: seq # A UI can allow something outside the list with
                      # widget: combo or if "other" is in this sequence
            sequence:
              - type: scalar
~~~
