


----------

Dear Chunxian,

I didnt find any document indicates that offlining/removing the overheated FRU within the 240-second window will explicitly cancel the pending chassis shutdown. The available code confirms the presence of thermal thresholds, chassis over-temperature shutdown logic, and FRU/FPC offline handling, but no direct logic was found stating that an offline/remove action aborts the timer once started.

The most reasonable is that such an action may help only if it clears the chassis over-temperature condition before timer expiry. However, this is not guaranteed, since thermal sensing/state refresh and cooldown are not instantaneous.

