# MVP-Proposition
MVP proposition base on https://github.com/googlesamples/android-architecture

## Summary :
Here a detail concrete implementation base on Android-architecture (https://github.com/googlesamples/android-architecture) 

#### Base Class :
We define a basic **Presenter** and **View** which will be extend by concrete MPV implementation

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
     * Handle all events coming from {@link BaseView}. Not a require but here we decided to define 
     * on generic function which will handle all event coming from the BaseView
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

#### Concrete MPV impl : EditContact

```java
public class EditContactView extends LinearLayout
        implements BaseView<EditContactPresenter> {

    @BindView(R.id.first_name) EditView mFirstName;
    @BindView(R.id.last_name) EditView mLastName;
    @BindView(R.id.phone) EditView mPhone;
    @BindView(R.id.submit) Button mSubmit;
    
    private EditContactPresenter mEditContactPresenter;

    public EditContactView(Context context) {
        super(context);
    }

    @Override
    protected void onFinishInflate() {
        super.onFinishInflate();
        ButterKnife.bind(this);
        ...
    }

    @Override
    public void setPresenter(@NotNull EditContactPresenter presenter) {
        mEditContactPresenter = presenter;
    }

    public void bindContent(Contact contact) {
        mFirstName.setText(contact.firstName);
        mFirstName.setOnClickListner(new OnClickListner() {
            public void onClick(View v) {
                mEditContactPresenter.onHandle(
                            BoardSearchSuggestionPresenter.KEYWORD_SELECTED, keywordSuggestion);
            }
        });
        mLastName.setText(contact.lastName);
        mPhone.setText(contact.phone);
        
        mSubmit..setOnClickListner(new OnClickListner() {
            public void onClick(View v) {
                Contact contact = getNewInfo()
                mEditContactPresenter.onHandle(
                            EditContactPresenter.ON_SUBMIT, contact);
            }
        });
    }
}
```

```java
public class EditContactPresenter implements BasePresenter {

    @IntDef({ON_EDIT_PHONE, ON_SUBMIT, ...})
    @Retention(RetentionPolicy.SOURCE)
    public @interface EventType{}
    
    private EditContactView mEditContactView;
    private Contact mContact;

    public EditContactPresenter( @NonNull EditContactView editContactView, @NonNull Contact contact) {
        mEditContactView = editContactView;
        mContact = contact;
    }
 
    @Override
    public void present() {
        editContactView.bindContent(mContact);
    }

    @Override
    public void clean() {
        editContactView.unbind();
    }

    @Override
    public void onHandle(@EventType int event, Object... params) {
        if (event == ON_EDIT_PHONE) {
            String keyword = (String) params[0];
            ...
        } else if (event == ON_SUBMIT) {
            Contact contact = (Contact) params[0];
            ...
        }
    }
}
```
