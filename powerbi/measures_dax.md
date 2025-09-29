# Key DAX Measures (Funnel)

Event1 Sessions :=
VAR _pv = VALUES ( PageViews[session_Id] )
VAR _ev1 =
    CALCULATETABLE (
        DISTINCT ( customEvents[session_Id] ),
        FILTER (
            customEvents,
            NOT ISFILTERED ( 'Event1'[name] )
                || customEvents[name] IN VALUES ( 'Event1'[name] )
        )
    )
RETURN COUNTROWS ( INTERSECT ( _pv, _ev1 ) )

Event2 Sessions (ANY) :=
VAR _pv = VALUES ( PageViews[session_Id] )
VAR _ev2 =
    CALCULATETABLE (
        DISTINCT ( customEvents[session_Id] ),
        FILTER (
            customEvents,
            NOT ISFILTERED ( 'Event2'[name] )
                || customEvents[name] IN VALUES ( 'Event2'[name] )
        )
    )
RETURN COUNTROWS ( INTERSECT ( _pv, _ev2 ) )

Event2 Sessions (per name) :=
VAR _pv = VALUES ( PageViews[session_Id] )
VAR _name = SELECTEDVALUE ( 'Event2'[name] )
VAR _ev2 = CALCULATETABLE ( DISTINCT ( customEvents[session_Id] ), customEvents[name] = _name )
RETURN COUNTROWS ( INTERSECT ( _pv, _ev2 ) )

Conversion % := DIVIDE ( [Event2 Sessions (ANY)], [Event1 Sessions] )
