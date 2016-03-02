# TrackChanges

TrackChanges is a C# .NET Revit add-in that tracks Revit BIM database modification by creating and comparing snapshots of element properties.

For more information, please refer
to [The Building Coder](http://thebuildingcoder.typepad.com) and
the detailed article
on [Tracking Element Modification](http://thebuildingcoder.typepad.com/blog/2016/01/tracking-element-modification.html).


## Enhancement

I am pondering an enhancement of this external command based add-in that I suggested to Tim Corneliussen in
the [Revit API discussion forum](http://forums.autodesk.com/t5/revit-api/bd-p/160) thread
on [dynamic model update after loading family](http://forums.autodesk.com/t5/revit-api/dynamic-model-update-after-loading-family/m-p/6052310):

Tim says he needs to track changes.

I suggested that he take a look at this.

**Response:** Your solution looks really impressive. I haven't had the chance to implement the main fundamentals in my project yet. As a starting programmer, the concept of hash code is still new to me but it looks like the right way to go. My main concern is how it will effect the perfomance of the routine.

The main purpose of my tool is that each addtition or modification will be registered by modifying some parameters including a "time-parameter" and "date-parameter". To do so, but correct me if I'm wrong, I need to trigger an event or use a DMU to determine when an element is added or modified.

Maybe I can use your snapshot technique combining it with a DMU. But doing so the DMU also collects a lot of data next to the snapshot routine. Is this necessary? Are there alternatives to affoid this sort of useless multiple data collecting?

Do elements themselves contain relevant information about their own creation or modification (perhaps a certain property that most people aren't aware of)? If so, I can use a single event (sort of like your suggestion on your blog), for example the DocumentSavingAs event. Last possible solution I can think of at the moment is a way to look even deeper in to the updaterdata/-information hoping it contains more general information about the addition or modification of the relevant elements.

Hoping that you'll understand the scenario I'm describing. For now I wil try to use the snapshot routine combining it with a DMU and a viewactivating event. Last mentioned will be used to determine whether another document becomes active (when a user has openened multiple projects). I will place an update when I have succesfully created a working solution to discuss the results with you fellow readers. If someone can tell me if this solution probably won't really work please do so.

**Answer:** Thank you very much for your appreciation.

I think the main characteristic of the modification tracker is simplicity, rather than impressiveness.

Of course, simplicity is much more impressive than impressiveness :-)

If you want to be notified on every single modification of an element and store that information immediately, then indeed you can and have to use either DMU or the DocumentChanged event.

The latter does not allow you to modify anything in the same transaction, though, whereas the former does.

If you want to guarantee that your date and time markers stored in Revit parameters are always up to date, immediately, then you need to use DMU.

But do you really need that?

You need to understand that DMU is complex and adds a significant burden to Revit, depending on how many elements trigger it, which in your case would be many.

Do you really need to keep track of the element modification on a split second-by-second basis?

Would it not be enough to track changes every minute, or every ten minutes?

If so, then you can vastly simplify your approach and vastly reduce the burden on Revit by using the modification tracker and completely avoiding DMU and the DocumentChanged event.

Just track changes based on snapshots taken every X minutes, for instance.

Regarding the issue of the hash code: that is a minor detail, and pretty irrelevant.

You can just store the full data. Depending on what criteria you use to define when an element has changed, you might need to store a lot of information for each element.

I suggested the hash code as a way to reduce and unify that data storage, but that has absolutely nothing to do with the fundamental concept.

To quote the original post: "We use the hash code to determine whether the state has been modified compared to a new element state snapshot made at a later time. We could obviously also store the entire original string representation instead of using a hash code. The hash code is small and handy, whereas the entire string contains all the original data. It is up to you to choose which you would like to use."

The hash code will not affect performance much, just reduce the memory used to cache the starting snapshot.

I would recommend thinking this through in depth and peace and quiet.

If you do not require split-second time slice data, I would avoid the DMU and DocumentChanged events, both, completely.

I am very much looking forward to hearing and discussing your further thoughts on this.



## Author

Jeremy Tammik,
[The Building Coder](http://thebuildingcoder.typepad.com) and
[The 3D Web Coder](http://the3dwebcoder.typepad.com),
[ADN](http://www.autodesk.com/adn)
[Open](http://www.autodesk.com/adnopen),
[Autodesk Inc.](http://www.autodesk.com)

## Contributors

- Jason Schaeffer [@joespiff](https://github.com/joespiff)


## License

This sample is licensed under the terms of the [MIT License](http://opensource.org/licenses/MIT).
Please see the [LICENSE](LICENSE) file for full details.
