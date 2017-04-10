# useful_drupal
A general list of useful Drupal programming structures

##### Get all query parameters
`\Drupal::request()->query->all()`

##### Get single query parameter
`\Drupal::request()->query->>get('paramKey')`

##### Load User By Id
`\Drupal\user\Entity\User::load(**$id**)`

##### Create User
`\Drupal\user\Entity\User::create()`

##### Get Current User
`\Drupal::currentUser()`

##### Get user by custom field
```
 $user_lookup = \Drupal::entityQuery('user')
      ->condition('field_user_first_name', 'Foo');
    $user_nids = $user_lookup->execute();
````

##### Check if user is autenticated or annonymous
`\Drupal::currentUser()->isAutheticated()`
`\Drupal::currentUser()->isAnnonymous()`



### Nodes
##### Get Current Node
`\Drupal::routeMatch()->getParameter('node')`


### Blocks
##### Create block programmatically
```
use Drupal\Core\Block\BlockBase;

class BlockClassName extends BlockBase {
    public function build() {
        // Code here...

        return array(
            '#type' => 'type',
            '#markup' => 'markup'
        );
    }
}
```

### Events
##### Kernel Events
```
KernelEvents::CONTROLLER; // The CONTROLLER event occurs once a controller was found for handling a request.
KernelEvents::EXCEPTION; // The EXCEPTION event occurs when an uncaught exception appears.
KernelEvents::FINISH_REQUEST; //The FINISH_REQUEST event occurs when a response was generated for a request.
KernelEvents::REQUEST; // The REQUEST event occurs at the very beginning of request dispatching.
KernelEvents::RESPONSE; // The RESPONSE event occurs once a response was created for replying to a request.
KernelEvents::TERMINATE; // The TERMINATE event occurs once a response was sent.
KernelEvents::VIEW; // The VIEW event occurs when the return value of a controller is not a Response instance.
```
