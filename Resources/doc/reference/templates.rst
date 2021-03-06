Templates
=========

By default, an Admin class uses a set of templates, it is possible to tweak the default values by editing the configuration

.. configuration-block::

    .. code-block:: yaml

        sonata_admin:
            templates:
                # default global templates
                layout:  SonataAdminBundle::standard_layout.html.twig
                ajax:    SonataAdminBundle::ajax_layout.html.twig
                dashboard: SonataAdminBundle:Core:dashboard.html.twig

                # default values of actions templates, they should extend global templates
                list:    SonataAdminBundle:CRUD:list.html.twig
                show:    SonataAdminBundle:CRUD:show.html.twig
                edit:    SonataAdminBundle:CRUD:edit.html.twig
                history: SonataAdminBundle:CRUD:history.html.twig
                preview: SonataAdminBundle:CRUD:preview.html.twig
                delete:  SonataAdminBundle:CRUD:delete.html.twig
                batch:   SonataAdminBundle:CRUD:list__batch.html.twig
                batch_confirmation: SonataAdminBundle:CRUD:batch_confirmation.html.twig

                # list related templates
                inner_list_row: SonataAdminBundle:CRUD:list_inner_row.html.twig
                base_list_field: SonataAdminBundle:CRUD:base_list_field.html.twig

                # default values of helper templates
                short_object_description: SonataAdminBundle:Helper:short-object-description.html.twig

                # default values of block templates, they should extend the base_block template
                list_block: SonataAdminBundle:Block:block_admin_list.html.twig

                # default values of pager templates
                pager_links: SonataAdminBundle:Pager:links.html.twig
                pager_results: SonataAdminBundle:Pager:results.html.twig


Usage of each template :

* layout : base layout used by the dashboard and an admin class
* ajax : default layout used when an ajax request is performed
* dashboard: default layout used at the dashboard
* list : the template to use for the list action
* show : the template to use for the show action
* edit : the template to use for the edit and create action
* history : the template to use for the history / audit action
* history_revision_timestamp : the template to use when rendering the timestamp of a given revision
* list_block : the template used for the list of admin blocks on the dashboard
* preview : the template to use for previewing an edit / create action
* short_object_description: used to represent the entity in one-to-one/many-to-one relations
* delete: the template to use for the delete action
* inner_list_row: the template to render a list row
* pager_links: the template to render the first, previous, next and last page links
* pager_results: the template to render the amount of pages, number of results and items per page

The default values will be set only if the ``Admin::setTemplates`` is not called by the Container.

You can easily extend the provided templates in your own template and customize only the blocks you need to change:

.. code-block:: jinja

    {% extends 'SonataAdminBundle:CRUD:edit.html.twig' %}
    {# Acme/MyBundle/Resources/view/my-custom-edit.html.twig #}

    {% block title %}
        {{ "My title"|trans }}
    {% endblock%}

    {% block actions %}
         <div class="sonata-actions">
             <ul>
                 {% if admin.hasroute('list') and admin.isGranted('LIST')%}
                     <li class="btn sonata-action-element"><a href="{{ admin.generateUrl('list') }}">{{ 'link_action_list'|trans({}, 'SonataAdminBundle') }}</a></li>
                 {% endif %}
             </ul>
         </div>
    {% endblock %}


.. code-block:: php

    <?php // MyAdmin.php

    public function getTemplate($name)
    {
        switch ($name) {
            case 'edit':
                return 'AcmeMyBundle::my-custom-edit.html.twig';
                break;
            default:
                return parent::getTemplate($name);
                break;
        }
    }


Row Template
------------

It is possible to completely change how each row of results is rendered in the
list view. For more information about this, see the :doc:`recipe_row_templates`
cookbook entry.
