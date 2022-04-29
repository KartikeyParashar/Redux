                                            CAKE SHOP
           Entities                                                             Activites
Shop - Stores cakes on a shelf                                           Customer - Buy a cake
Shopkeeper - At the front of the store                                   Shopkeeper - Remove a cake from the shelf
Customer - At the store entrance                                                    - Receipt to keep track

###########################################################################################################################

         Cake Shop Scenerio                         Redux                        Purpose
         ------------------                         -----                        -------
    1.   Shop                                       Store                 Holds the state of your application
    2.   Intention to BUY_CAKE                      Action                Describes what happened
    3.   Shopkeeper                                 Reducer               Ties the store and actions together


@@  A STORE that holds the state of your application.
@@  An ACTION that describes the changes in the state of the application.
@@  A REDUCER which actually carries out the state transition depending on the action.    

############################################################################################################################

THREE PRINCIPLES
----------------
1. "The state of your whole application is stored in an object tree within a single store"
   (Maintain our application state in a single object which would be managed by the Redux Store)
   Cake Shop Example - Let's assume we are tracking the number of cakes on the shelf
   {
       numberOfCakes: 10
   }

2. "The only way to change the state is to emit an action, an object describing what happened"
   (To update the state of your app, you need to let Redux know about that with an action)
   (Not allowed to directly update the state object)
   Cake Shop Example - Not allowed to directly take the cake from the shelf. Let the shopkeeper know about
                       our action - BUY_CAKE
   {
       type: BUY_CAKE
   }

3. "To specify how the state tree is transformed by actions, you write your pure reducers"
    Cake Shop Example - Reducer is the shopkeeper

    const reducer = (state, action) => {
        switch (action.type) {
            case BUY_CAKE: return {
                numberOfCakes: state.numberOfCakes - 1
            }
        }
    }


                                    THREE PRINCIPLES OVERVIEW
                                    -------------------------
                         subscribed
                   -------------------->  Javascript APP  ------------------------>
                   |                                                               |
                   |                                                               | dispatch
                   |                                                               |
                   |                                                               |
             REDUX STORE ( State )                                               ACTION
                   |                                                               |
                   |                                                               |
                   |                                                               | (We can use middleware here!)
                   |                                                               |
                    <---------------------  Reducer  <------------------------------                                         

#############################################################################################################################

ACTIONS
-------
a) The only way your application can interact with store
b) carry some information from your app to the redux store
c) Plain Javascript objects
d) Have a "type" property that indicates the type of action being performed
e) The "type" property is typically defined as string constants

REDUCERS
--------
a) Specify how's the app state changes in response to actions sent to the store
b) Function that accepts state and action as arguments, and returns the next state of the application
EXAMPLE - ( previousState, action) => newState

#######################################################################################################################

REDUX STORE
-----------
One Store for Entire application
Responsibilities:
a) Holds the application state
b) Allow access to state via getState()
c) Allow state to be updated via dispatch(action)
d) Registers listeners via subscribe(listener)
e) Handles unregistering of listeners via the function returned by subscribe(listener)

#########################################################################################################################

Cake Shop
Cakes Stored on the shelf
Shopkeeper to handle BUY_CAKE from Customer

Now, Shopkeeper want to expand business and add a new shopkeeper to sell Ice Creams

Sell Ice Creams!
Ice Creams stored in the freezer
New Shopkeeper to handle BUY_ICECREAM from Customer

##########################################################################################################################
Middlewares
----------
a) It is the suggested way to extend Redux with custom functionality
b) Provides a third party-extension point between dispatching an action, and the moment it reaches the reducer
c) Use middleware for logging, crash reporting, performing asynchronous tasks etc. 

##########################################################################################################################

----------------------------------------------Synchronous actions---------------------------------------------------------
As soon as an action was dispatched, the state was immediately updated.
If you dispatch the BUY_CAKE action, the numOfCakes was right away decremented by 1.
Same with BUY_ICECREAM action as well.

---------------------------------------------Async Actions----------------------------------------------------------------
Asynchronous API calls to fetch data from an end point and use that data in your application.


--------------------------------------------Our Application---------------------------------------------------------------
Fetches a list of users from an API end point and stores it in redux store.

@@@@ State ? @@@@
state = {
    loading: true, // Display a loading spinner in your component
    data: [], // List of Users
    error: "" // Display error to the user 
}

@@@@ Actions ? @@@@
FETCH_USERS_REQUEST --> Fetch list of users
FETCH_USERS_SUCCESS --> Fetched Successfully
FETCH_USERS_FAILURE --> Error fetching the data

@@@@ Reducer ? @@@@
case: FETCH_USERS_REQUEST
      loading: true

case: FETCH_USERS_SUCCESS
      loading: false
      users: data { from API }

case: FETCH_USERS_FAILURE
      loading: false
      error: error { from API }