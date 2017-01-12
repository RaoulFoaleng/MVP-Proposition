# MVP-Proposition
MVP proposition base on https://github.com/googlesamples/android-architecture

# H2 Summary :
Here a detail concrete implementation base on Android-architecture (https://github.com/googlesamples/android-architecture) 

# H4 Base Class :

```java
/***
 * MVP architecture.
 * A basic presenter representation in the model view controller pattern
 */
public interface BasePresenter {

    /**
     * All the business logic to inflate and display the view should be implement here
     */
    void present();

    /**
     * Use it to clean up the view, model object
     */
    void clean();

    /**
     * Handle all events coming from {@link BaseView}
     * @param event : the event type
     * @param params : the event parameters
     */
    void onHandle(int event, Object... params);
}
```

```java
/***
 * MVP architecture
 *  * A basic view representation in the model view presenter pattern
 * @param <T>
 */
public interface BaseView<T extends BasePresenter> {

    /**
     * Set the presenter object.
     * This reference should be used to propagate event to the presenter
     * @param presenter
     */
    void setPresenter(T presenter);
}
```
