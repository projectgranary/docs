# Segment Store

![Data flow within data-out zone of Granary](../../../.gitbook/assets/dataflow_out.PNG)

The Segment Store holds information derived from the Event Store or the Profile Store. Such a derivative can be a selection of events or profiles and is called a **segment**. It may also comprise a projection of Event or Profile fields. Thus, a segment's main purpose is to map data from Granary on to a desired targed schema for a data-consuming downstream application.

Technically, a segment is a table or a view in a database. Each tuple in that table represents an event or a profile, comprising selected and projected fields. Note that for profiles, the segments are relational representations of the nested Profile JSON data structure. For events, the segments merely comprise selections and projections.

The subpages describe segment definition and deployment.

