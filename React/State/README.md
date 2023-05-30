# Thoughts on Various State Management Tools

## Redux vs Context API

Redux is a robust yet quite complex tool, one of its central tenets being the use of a single global store. However, it's worth noting that the effectiveness of this tool mainly comes to the fore when your application requires a complex, comprehensive store.

In many scenarios, you may not need to maintain a vast amount of globally accessible data within your application. Typically, this includes user information, shopping cart functionalities, and other similar non-complex elements. In such situations, the Context API emerges as a more fitting choice.

However, a single global store could be valuable in certain instances. Consider a situation where you have about ten pages, each making an API request to fetch data for rendering. If you can modify this data by submitting a form from the page, navigating away and then returning might not necessitate another API request.

Nonetheless, if the data is susceptible to edits by others, the page's information may become outdated. In such cases, you would need a strategy to determine when to refresh the data.

Also, it's worth noting that Redux is not recommended for use in forms. So any data modification in forms requires either a local state or another tool such as Context API or MobX.

### Given this, my conclusion is that a global store is beneficial when:

#### You need to maintain a significant volume of data in the global store, accessible from any part of your app.
**Examples**:
1. A large application with few or no routes, and numerous places where you can modify data.
2. Site builders where you have forms for data editing on one side of the viewport and the site preview on the other (in this scenario, considering MobX could be beneficial).
#### You need to cache GET requests AND only one user has the authority to modify this data.
**Examples**:
1. User's profile.
2. Cases where only a single administrator has access to the data.
