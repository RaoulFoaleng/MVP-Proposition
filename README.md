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
#### Concrete MPV impl : BoardSearchSuggestion
```java
public class BoardSearchSuggestionView extends LinearLayout
        implements BaseView<BoardSearchSuggestionPresenter> {

    @BindView(R.id.title) BrioTextView _title;
    @BindView(R.id.board_suggestion_recycler) RecyclerView _recyclerView;
    
    private BoardSearchSuggestionPresenter _boardSearchSuggestionPresenter;
    private BoardSearchSuggestionAdapter _adapter;

    private BoardSearchSuggestionAdapter.OnItemClickListener _onItemClickListener =
            new OnItemClickListener() {
                @Override
                public void onItemClick(KeywordSuggestion keywordSuggestion) {
                    _boardSearchSuggestionPresenter.onHandle(
                            BoardSearchSuggestionPresenter.KEYWORD_SELECTED, keywordSuggestion);
                }
            };

    public BoardSearchSuggestionView(Context context) {
        super(context);
    }

    @Override
    protected void onFinishInflate() {
        super.onFinishInflate();
        ButterKnife.bind(this);

        _layoutManager = new LinearLayoutManager(context);

        _adapter = new BoardSearchSuggestionAdapter(context, _colors, _onItemClickListener);
        _recyclerView.setAdapter(_adapter);
        ...
    }

    @Override
    public void setPresenter(@NotNull BoardSearchSuggestionPresenter presenter) {
        _boardSearchSuggestionPresenter = presenter;
    }

    public void bindContent(BoardSearchSuggestionData data) {
        setTitle(data.getTitle());
        _adapter.setItems(data.getKeywordSuggestions());
    }

    private void setTitle(String title) {
        _title.setText(title);
    }
}
```

```java
public class BoardSearchSuggestionPresenter implements BasePresenter {

    private BoardSearchSuggestionView _boardSearchSuggestionView;
    private BoardSearchSuggestionData _boardSearchSuggestionData;

    public BoardSearchSuggestionPresenter(
            @NonNull BoardSearchSuggestionView boardSearchSuggestionView,
            @NonNull BoardSearchSuggestionData boardSearchSuggestionData) {
        _boardSearchSuggestionView = boardSearchSuggestionView;
        _boardSearchSuggestionData = boardSearchSuggestionData;
    }

    @Override
    public void present() {
        _boardSearchSuggestionView.bindContent(_boardSearchSuggestionData);
    }

    @Override
    public void clean() {
        _boardSearchSuggestionView.unbind();
    }

    @Override
    public void onHandle(int event, Object... params) {
        if (event == KEYWORD_SELECTED) {
            KeywordSuggestion keywordSuggestion = (KeywordSuggestion) params[0];
            handleKeywordSelected(keywordSuggestion);
        }
    }

    private void handleKeywordSelected(KeywordSuggestion keywordSuggestion) {
        ...
    }
}
```
