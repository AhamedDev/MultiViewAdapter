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

To use the above sanpshot version add the following to your project's gradle file

```gradle
allprojects {
    repositories {
        maven {
            url 'https://oss.jfrog.org/artifactory/oss-snapshot-local'
        }
    }
}
```

## Basic Usage


Lets create an adapter which displays a list of cars. This the expected output for us :

> TODO Add image here

1. You need to create an ItemBinder for your model. ItemBinder is reponsible for creating and binding your view holders. Following is the code snippet of ItemBinder for CarModel class.

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

      // Register Binder
      adapter.registerBinders(new CarItemBinder());

      // Create Section and add items
      ListSection<YourModel> listSection = new ListSection<>();
      listSection.addAll(cars);

      // Add Section to the adapter
      adapter.addSection(listSection);

      // Now set the adapter to the recyclerview
      recyclerView.setAdapter(adapter);
  }
}
```

> Creation of ItemBinders and using a ListSection might feel like overwhelming step. But this gives you enough flexibility to re-use the ItemBinders across your app. Since MultiViewAdapter can have multiple sections and itembinders these steps are needed.

For advanced usage and features kindly take a look at sample app code. Also you can learn more from the extensive documentation. Take a look at [Wiki home](https://github.com/DevAhamed/MultiViewAdapter/wiki).

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
