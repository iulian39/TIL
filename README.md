# TIL

- [GIT](#git)
  * [Ammend](#ammend)
  * [Change remote link](#change-remote-link)
- [Docker](#docker)
  * [Starting a docker-compose.yml file](#starting-a-docker-composeyml-file)
  * [Building a docker image](#building-a-docker-image)
- [Security](#security)
- [CORS](#cors)
- [React](#react)
  * [React HOC](#react-hoc)
  * [React children](#react-children)
    + [Other](#other)
  * [Redux Middleware](#redux-middleware)
  * [Redux Selector](#redux-selector)
  * [Redux thunk](#redux-thunk)

## GIT

`git checkout -b NEWBRANCH`

`git push --set-upstream origin BRANCHNAME`

`git reset origin --hard`

`git pull origin master` # fetch and merge in one operation when you are on your branch

`git stash`

`git stash pop`

Oppsite of `git a .` -> `git reset .`

`git reset HEAD^` -> undo commit and get the files back

`git checkout master -- <file name>` -> revert certain file with the master's version

##### Note: Any changes not committed will be lost.

`git branch newbranch`      # Create a new branch, saving the desired commits

`git reset --hard HEAD~3`   # Move master back by 3 commits (Make sure you know how many commits you need to go back)

`git checkout newbranch`    # Go to the new branch that still has the desired commits

### Ammend
`git commit --amend `

`:wq`

`git push -f ...  `

### Change remote link 
`git remote -v`

`git remote set-url origin NEW_URL`

`git pull # to verify`

### Get changes from another branch to master without commiting
You can use `git diff <another-branch> ^HEAD` to print a diff of the changes that are in "another-branch", but not in your current branch (HEAD). And then apply those changes to the current index by passing them to `git apply -`

`git diff <another-branch> ^HEAD | git apply -`

## Docker
### Starting a docker-compose.yml file
`docker-compose pull`

`docker-compose up -d`

### Building a docker image
`docker build . -t NAME`

`docker run -p 8070:8080 NAME(Might also be a  url)`

Then you can see it on  http://localhost:8070/Index.html

## Security
`SELECT * from users where username='admin' and (CASE WHEN (substr(key, 1, 1) = 'a') then LIKE('ABCDEFG',UPPER(HEX(RANDOMBLOB(200000000)))) else 1 end); --`
this basically checks if there is the specified letter in the key at a given position. if there is, we do some operation (use sleep if not sqlite3)

## CORS
For front-end, header  { crossdomain: true }
For back-end, Access-Control-Allow-Origin in response

## React
### React HOC
In simple terms, we could say that a Higher Order Component is a function that accepts a component as parameter and returns another component that wraps it.

`export default HOC(App);`
### React children

`<Parent>   <p> Child </p>  </Parent>`
  in parent -> this.props.children
  
With React.Children we can map the children: 
  const buttons = Children.map(this.props.children, child => {( <button> {child} </button>)};
  #### Other
- React.Children.only (expects to receive a single element, used for render)
- React.Children.count (return nr of children)
- React.Children.toArray (this.props.children is not an array)
### Redux Middleware 
Action that happens between the action and the reducer (might be more than 1 middleware)(ex: console.log of action, confirmation for deletion)
```js
// store => next => action = curried function
const loggerMiddleware = store => next => action => {
  console.log('dispatching: ', action);
  next(action); // pass to the next middleware/reducer
}

const confirmationMiddleware = store => next => action => {
  if(action.shouldConfirm){
    if(confirm('Are you sure?')){
      next(action);
    }
    // no next(action) -> nothing happens
  } else {
    next(action);
  }
}

const promiseMiddleware = store => next => action => {
  if(action.promise){
    action.promise
    .then(rawResponse => rawResponse.json())
    .then(response => store.dispatch({type: action.type, payload: response}));
  } else {
    next(action);
  }
}

const store = createStore(
  reducer,
  applyMiddleware(loggerMiddleware, confirmationMiddleware, promiseMiddleware)
);
```
### Redux Selector

```js
export const selectUserReservation = (state) => state.auth.user.reservation;
export const selectRoomList = (state) => state.rooms.list;
export const selectUserRoom = createSelector(
  selectUserReservation, selectRoomList, // takes the above functions
  (userReservation, roomList) => roomList.find(room => room.id === userReservation.roomType)
  // The results of each of the selectors
);
```
### Redux thunk
```js
export default createStore(
  reducer, 
  applyMiddleware(thunk.withExtraArgument(api)) // we can access dispatch, getState and also api
);

// in action
export const requestBook = id => (
  (dispatch, getState, api) => {
    dispatch(entryLoading(id));
    // const user = getState().user;     
    return api.fetchBook(id)
    .then(book => {
      dispatch( entryLoaded(book) );
      return book;
    })
    .catch(err => {
      dispatch( entryLoadError() );
    });
  }
);

export const requestBookAndSimilars = (id) => ( // elegant way to dispatch the 2nd action
  dispatch => {
    dispatch(requestBook(id)).then(book => dispatch(requestBooksByTags(id, book.tags)));
  }
)

```
