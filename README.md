![Alt text](images/cover.png)

[![GitHub license](https://img.shields.io/badge/license-Apache--2.0-blue.svg)](https://github.com/DevAhamed/MultiViewAdapter/blob/master/LICENSE)
[![Code coverage](https://codecov.io/gh/DevAhamed/MultiViewAdapter/branch/2.x/graph/badge.svg)](https://codecov.io/gh/DevAhamed/MultiViewAdapter)
[![Build Status](https://app.bitrise.io/app/7f9137a2f1df08c1/status.svg?token=bm8ERviCGI3BrqG_AEo9sA)](https://www.bitrise.io/app/7f9137a2f1df08c1)

## What?

Recyclerview is one of the powerful widgets inside android framework. But creating adapters with multiple view types is always exhausting. Not anymore. MultiViewAdapter makes it easy for you to create adapter with multiple view types easily. Using the library you will be able to composable view holders, which can re-used across your app. Apart from this MultiViewAdapter adds many other most used features into the library as add-ons.

## Gradle Dependency

The Gradle dependency is available via JCenter. JCenter is the default maven repository used by Android Studio. The minimum API level supported by this library is API 14.

#### Adding Library

[![Core](https://api.bintray.com/packages/devahamed/MVA2/adapter/images/download.svg)](https://bintray.com/devahamed/MVA2/adapter/_latestVersion)

```gradle
dependencies {
    implementation 'dev.ahamed.mva2:adapter:2.0.0-alpha01'
}
```

#### Adding Extension

```gradle
dependencies {
    implementation 'dev.ahamed.mva2:ext-data-binding:2.0.0-alpha01' // DataBinding
    implementation 'dev.ahamed.mva2:ext-decorators:2.0.0-alpha01'   // Decorators
    implementation 'dev.ahamed.mva2:ext-rxdiffutil:2.0.0-alpha01'   // RxDiffUtil
}
```

#### Using Snapshot Version

Just add '-SNAPSHOT' to the version name

```gradle
dependencies {
    implementation 'dev.ahamed.mva2:adapter:2.0.0-SNAPSHOT' // Library
}
```

To use the above snapshot version add the following to your project's gradle file

```gradle
allprojects {
    repositories {
        maven {
            url 'https://oss.jfrog.org/artifactory/oss-snapshot-local'
        }
    }
}
```

## Core Concepts

Core mantra of MultiViewAdapter - Separation of view logic from data management logic. You get two different components which are :

1. Section - Component to hold your data. All data related updates will run here and proper notify method will be called on the adapter.
2. ItemBinder - Component which creates and binds your view holder. All view related logic should go here.


#### Section

Section is the building block for MultiViewAdapter. Section will hold your data which needs to be displayed inside the recyclerview -  data can be a single item or list of items. When the underlying data is changed the section will calculate the diff and call the correct notify method inside the adapter. You can add as many as Section to an adapter.

![How Section Works GIF](images/how-section-works.gif)

There are different types of sections.

|Name|Description|
|---|---|
|ItemSection|Section to display single item|
|ListSection|Section to display list of items|
|HeaderSection|Section to display list of items along with a header|
|NestedSection|Section which can host other section|
|TreeSection|Section which can display items in tree fashion. Ex: Comment list|

#### ItemBinder

ItemBinder is the class where all view related code should be written. ItemBinder is responsible for creating and binding view holders. The method signatures are kept close to the default ```RecyclerView.Adapter``` method signatures. For each viewtype inside the recyclerview, you need to create an ItemBinder. 

ItemBinder allows you to have different decoration for different view types. Apart from this, by using an ItemBinder you will be able to add Swipe-to-dismiss, drag and drop features.


## Basic Usage


Lets create an adapter which displays a list of cars. Follow these steps.

1. You need to create an ItemBinder for your model. ItemBinder is responsible for creating and binding your view holders. Following is the code snippet of ItemBinder for CarModel class.

<b>CarBinder</b>
 
```java
public class CarBinder extends ItemBinder<CarModel, CarBinder.CarViewHolder> {

  @Override public CarViewHolder createViewHolder(ViewGroup parent) {
      return new CarViewHolder(inflate(R.layout.item_car, parent));
  }

  @Override public boolean canBindData(Object item) {
      return item instanceof CarModel;
  }

  @Override public void bindViewHolder(CarViewHolder holder, CarModel item) {
      holder.tvCarName.setText(item.getName());
  }

  static class CarViewHolder extends ItemViewHolder<CarModel> {
    
    TextView tvCarName;

    public CarViewHolder(View itemView) {
        super(itemView);
        tvCarName = findViewById(R.id.tv_car_name);
    }
  }
}
```

2. Now create an adapter and use the ItemBinder created above. Since we are displaying a list of items we need to create an ListSection object and add the data items to it. Add the section to adapter. Done.

<b>In your Activity/Fragment</b>

```java
class CarListActivity extends Activity {
  private RecyclerView recyclerView;
  private List<CarModel> cars;

  public void initViews() {

      // Create Adapter
      MultiViewAdapter adapter = new MultiViewAdapter();
      recyclerView.setAdapter(adapter);

      // Register Binder
      adapter.registerBinders(new CarItemBinder());

      // Create Section and add items
      ListSection<YourModel> listSection = new ListSection<>();
      listSection.addAll(cars);

      // Add Section to the adapter
      adapter.addSection(listSection);
  }
}
```

## Changelog
See the project's Releases page for a list of versions with their changelog. [View Releases](https://github.com/DevAhamed/MultiViewAdapter/releases)<br/>
If you watch this repository, GitHub will send you an email every time there is an update.


## Alternatives
If this library does not suit your needs create an issue/feature request. Mean while check these awesome alternatives as well.

1. [MultipleViewTypesAdapter](https://github.com/yqritc/RecyclerView-MultipleViewTypesAdapter) - Original inspiration for this library.<br/>
2. [AdapterDelegates](https://github.com/sockeqwe/AdapterDelegates)
3. [Groupie](https://github.com/lisawray/groupie)
4. [Epoxy](https://github.com/airbnb/epoxy)


## License
```
Copyright 2017 Riyaz Ahamed

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
