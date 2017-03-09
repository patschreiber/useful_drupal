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
