# Binders for Views in android #

Creating Adapters again and again becomes boring, and there are things that are redundant, so why not let machine to the boring stuff and us write what's important?

So code generation comes to the rescue.

Let's say we have sample.xml view in res/layout folder. The view has one TextView field with +@id/name

what you get is:

```
//...
public class Binders
{

	private static Binders _instance = new Binders();

	public static Binders instance(){
		return _instance;
	}


	public Pair<Sample,View> getSample(View view, Context context, ViewGroup parent){
		if(view == null){
			LayoutInflater inflater = LayoutInflater.from(context);
			view = inflater.inflate(R.layout.sample,parent,false);
            		Sample tag = new Sample(view);
            		view.setTag(R.layout.sample,tag);
            		return new Pair<Sample,View>(tag,view);
        	}
        	return new Pair<Sample,View>((Sample) view.getTag(R.layout.sample),view);
	}

	public Sample getSample(View view){
		Sample tag = (Sample)view.getTag(R.layout.sample);
		if(tag == null){
			tag = new Sample(view);
			view.setTag(R.layout.sample, tag);
		}
		return tag;
	}

	public class Sample
	{

		public TextView name;

		public Sample(View view){

			this.name = (TextView)view.findViewById(R.id.name);

		}

	}


```

You add adapters.tt and template.jar to your project and execute it

In that file change the package to the one you use in your application.

```
%JAVA_HOME%/bin/java -jar template.jar adapters.tt=gen2/android-binders/com/google/code/Binders.java
```

or add it to your ant build file.

```

  <target name="init" description="Build initialization">
    <mkdir dir="${module.basedir}/gen2/android-binders/com/google/code" />
    <exec executable="${project.jdk.bin}/java">
	<arg value="-jar" />
	<arg value="template.jar" />
	<arg value="adapters.tt=gen2/android-binders/com/google/code/Binders.java" />
    </exec>
  </target>

```

Set gen2 folder to be used as generated code folder

https://edd30187-a-62cb3a1a-s-sites.googlegroups.com/site/androidbinders/added_gen.png?attachauth=ANoY7cox78Da9OyNPvqpdFkLXOdcYkb-uw9fI3awzMNZnTRe6jK7N7EvqVIHOIKKbmJWrN10QYI1t17SMOnLynrse5eF-ANiRmoiQ5AAGwdZSY36cAngzXL6Ewg9ZWcZ-H8WtyUptdCAK1WZGSxD5Z3FGfW_5gwHFRPnMNnd0npjM3z17jypEQsfYX_EJTLru33Vx8oijc_IC4J_BruId_TGGzgxYwClZA%3D%3D&attredirects=0