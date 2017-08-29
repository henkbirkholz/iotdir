# IoT-DIR Early Review of I-D.ietf-opsawg-mud-08

## Draft Summary

This drafts defines a canonical way to compose a URI that points to a specific
resource called a MUD file. A MUD file is a text resource that contains
imperative guidance in the form of YANG-based ACL policies represented in JSON.
The imperative guidance is intended to be applied to things that can be
identified by the segments of the MUD URI via a specific controller. The version
of MUD is a component (segment) of the MUD URI and there are three examples in
what to embed a MUD URI (DevID, DHCP, and LLPDU) in.

## Comments on General Topics

This draft could be a significant step towards to self-descriptiveness of
things. Constrained things might require imperative guidance or declarative
guidance in order to be managed appropriately – which in includes aspects, such
as isolation, clustering, service/capability discovery/exposure, and therefore
security automation in general. In a lot of usage scenarios, it might not be
feasible to store that guidance on the thing itself and MUD URI could be a
solution to support a lot of vendor supplied information, such as reference
integrity measurements, intended composition of a composite devices, etc.

### Scope of Application

On one hand, this drafts limits the potential usage of the MUD architecture to
the use of one specific branch of YANG modules regarding ACL policies. There is
no way to express other manufacture usage description "information-types" or
"content-types" on a fundamental level and section 13 specifically states that
"coupled with the fact that we have also chosen to leverage existing mechanisms,
we are left with no ability to negotiate extensions and a limited desire for
those extensions in any event."

Creating augments for the metainfo container as it is also described in Section
13, on the other hand, allows for virtually any kind of information to be
expressed and conveyed via a MUD file, but on a semantic level that seems to be
a bit perplexing. 

This comment is not intended to question the feasibility of the technical
approach – which is okay –  but more taking into account the principle of least
surprise. Hence, a strong proposal – in respect to the already included
universal extension mechanism – to consider:

* aligning ACL content and "other" content on the same semantic level in the
  YANG module, and
* maybe indicating the corresponding semantics of the MUD file in the MUD URI
  itself.

### Intended & Allowed Representations/Formats

The assumption is that neither the MUD controller nor the server serving the MUD
files are constrained devices. The things that the imperative guidance is
intended to address can be constrained devices. YANG modules are used to create
the MUD files and the representation in the MUD files is JSON.

In the scope of the extensibility feature highlighted, is it intended that every
MUD file must contain content that relates to the structure of a YANG module (or
are there other data models for data at rest planned for)?

Is JSON intended to be the only allowed representation used for the
representation of content in MUD files (a question in respect to the CBOR-based
CoMI draft in the CORE WG)?

### Segment "mud-rev"

There seems to be conflicting definitions about the semantic of the segment
"mud-rev":

Section "5. What does a MUD URL look like?" states that a "mud-rev signifies the
version of the manufacturer usage description file" and "this memo specifies
"v1" of that file". The passages quoted here seem to imply that mud-rev is about
the instance of JSON that is the content of the MUD file.

Section "13. Extensibility" though states "at a coarse grain, a protocol version
is included in a MUD URL. This memo specifies MUD version 1. Any and all changes
are entertained when this version is bumped.", which implies that mud-rev is
intended to state the version of the MUD protocol and probably the respective
YANG module(s).

Probably, you want both? Most certainly, this has to be clarified.

### Segment "model"

Every device typically is a composite. Similarly, the hardware device-model
identifier is a potential composite of device-type, device-model &
device-version (and probably even more). The text is very vague on this part,
maybe deliberatively so because the authors do not want to prescribe how to
compose the string that constitutes the model segment – but a bit more guidance
on what this typically means and what could be caveats if you concatenate this
potentially very complex identifier into a single string is strongly
recommended.

Also, model is supposed to include a not further specified way to identify the
"version" of the software next to the "version" of the hardware. Even in very
small things, software components can be composites, too. Furthermore, a
software version may run on more than one device-model or even device-type.
While it might introduce complexity in respect to URI composition (one/two extra
segments), separating the hardware composite-identifier from the
software/firmware composite-identifier will be helpful and provide more clear
semantics to both humans and machines. 

### Query "extra"

While this option is included in the composition guidance of a MUD URI, it is
not included in the text anywhere. A guess of its purpose could be to provide
subsets of the modules via xpath or subtree expressions. Is that the case?
Additionally, including a variable in an immutable container, e.g. DevID, seems
to negate the intend of it being a variable? Maybe this should be highlighted in
the upcoming section that illustrates what the "extra" query option is for?

## Nits

What does the expression "relative to XML"  intends to convey in "JSON is used
as a serialization for compactness and readability, relative to XML."?
