+++
date = "2008-08-14T18:56:00-05:00"
title = "CherryPy, SQLAlchemy, and URI to SQL method"
+++

Ok some basic info first. I have sqlalchemy objects defined. I use cherrypy and have been writing methods for doing queries to my tables in the various means ive learned how over the past couple years. that is to say,

`/show_actor?id=4&production_id=2`

I keep looking at this code and thinking, this just isn't right, i was even recently looking at the turbogears2 wiki example (getting ideas for decorator usage and such) and seeing similar code, Again thinking, this just isn't right. Then somewhere between thinking and writing some code, i ended up halfway through the following and dont know how i got there. Basically its a uri to sql method. I make use of cherrypy's default method to traverse the uri that gets passed as *args ... well traverse it as in translate it into traversing my sql tables. Its all very short and concise i think.

I got the first draft of my uri to sql done, it goes something like this,

```
@expose
def default(self, *args, **kwargs):
    for n in args:
        if n in globals():
            query=Session.query(globals()[n])
    for k,v in make_filters(args):
        query=query.filter(globals()[k].name==v)
    yield json(query.all())
```

and uses this outside function

```
def make_filters(uri):
    for n in range(len(uri)):
        if not n%2 and n+1 < len(uri):
            yield [uri[n], uri[n+1]]
```

Basically now, the args passed to my default method let me traverse my sql tables like it was a directory tree. So you might say, i can open my folder Productions and find "aa" in there, and then i can open my Capture data related to "aa" and find MC001, or go to my Actor folder and get all my actor files such as "Jon" all accessible via

`/show/Production/aa/Capture/MC001`

or

`/show/Production/aa/Actor/Jon`

then using keyword arguments like

`/show/Production/aa/Actor/Jon?type=json`

i can specify special conditions or whatever and ive totally eliminated this really lame reiterative code of multiple methods for calling different data like /show_actor?prod_id=1, or calls with multiple if statements for handling what im asking for and its totally recursive no matter how deeply nested my tables are.

Im sure theres tons of problems and some shortsightedness here, and im still a relative newb to python (going on 6 months of use now i think) but anyway, i really dig this. Now I can start coding my ajax app to grab this or grab that and as I define tables through sqlalchemy, i will automatically be able to access my data.

Some things I need/want to do now is to provide custom filter options via keyword arguments and some sort of deliver data as type .. w/e, json, xml, yaml, yadda yadda yadda

Also to note, i call a json method in my code, thats part of a (probably overly complicated) generator i wrote that takes an sqlalchemy result object and produces a usable dict that gets dumped to json. It needs some extra work I think to be more useful but does what i need it to for now. Heres the code for that

```
def gmap(obj):
    for item in obj.__dict__.items():
        if item[0][0] is '_':
            continue
        if isinstance(item[1], unicode):
            yield [item[0], str(item[1])]
        else:
            yield item

def json(obj):
    if isinstance(obj, list):
        return simplejson.dumps( map(lambda x: dict(x), map(lambda x: gmap(x), obj)) )
    else:
        return simplejson.dumps( dict(gmap(obj)) )
```

Thanks to google's advanced python talk on video.google for breaking down some key concepts on generators and such.
