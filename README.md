# useful_drupal
A general list and cheatsheet of useful Drupal programming structures and "gotchas."

### Querying
##### Get all query parameters
`\Drupal::request()->query->all()`

##### Get single query parameter
`\Drupal::request()->query->get('paramKey')`

### Users
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

##### Get Full User object
`$current_user_entity = User::load(\Drupal::currentUser()->id());`


### Nodes
##### Get Current Node
`\Drupal::routeMatch()->getParameter('node')`

##### Get Node By Field
```
$nodes = \Drupal::entityTypeManager()
  ->getStorage('node')
  ->loadByProperties(array('field_machine_name' => $field_machine_name));
```

##### Get All Entity References (returns array)
```
$referenced_entities = $node->get('field_entity_ref')->referencedEntities();
```

##### Get Referenced Object (Node, as an example)
```
    $referencedEntity = $association
      ->get('field_entity_ref')
      ->first()
      ->get('entity')
      ->getTarget()
      ->getValue();
```


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

### Redirects
```
return $this->redirect('<front>');
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

### Queues
##### Run a queue from drush. 
Accepts 1 argument, queue_name, and 1 option, --time-limit (in seconds)
```
drush -l environment queue-run queue_name --time-limit=seconds
```
Example:
```
drush -l local queue-run update_users --time-limit=600
```

### Config
##### Run 'install' config on an already-installed module
Navigate to the devel tab on the admin site, then click the 'Execute PHP' tab.
```
$config_installer = \Drupal::service('config.installer');
$config_installer->installDefaultConfig('module', 'mymodule_name');
```

### Gotchas
#### View Templates
All views templates can be overridden with a variety of names, using the view, the display ID of the view, the display type of the view, or some combination thereof.

For each view, there will be a minimum of two templates used. The first is used for all views: views-view.html.twig.

The second template is determined by the style selected for the view. Note that certain aspects of the view can also change which style is used; for example, arguments which provide a summary view might change the style to one of the special summary styles.

The default style for all views is views-view-unformatted.html.twig.

Many styles will then farm out the actual display of each row to a row style; the default row style is views-view-fields.html.twig.

Here is an example of all the templates that will be tried in the following case:

View, named foobar. Style: unformatted. Row style: Fields. Display: Page.

```
views-view--foobar--page.html.twig
views-view--page.html.twig
views-view--foobar.html.twig
views-view.html.twig
views-view-unformatted--foobar--page.html.twig
views-view-unformatted--page.html.twig
views-view-unformatted--foobar.html.twig
views-view-unformatted.html.twig
views-view-fields--foobar--page.html.twig
views-view-fields--page.html.twig
views-view-fields--foobar.html.twig
views-view-fields.html.twig
```
Important! When adding a new template to your theme, be sure to flush the theme registry cache!

##### More Information
https://api.drupal.org/api/drupal/core%21modules%21views%21views.theme.inc/group/views_templates/8.2.x

#### Return Values 
```getValue()``` returns an array.
```->value``` returns field value.

#### Entity Field Names
```$entity->get('field_name')->getValue();``` is synonymous with ```$entity->field_name->getValue();```.

#### Check Page Status Code
```
$status = \Drupal::requestStack()->getCurrentRequest()->attributes->get('exception');
if ($status && $status->getStatusCode() == 404){ 
  //... 
}
```
