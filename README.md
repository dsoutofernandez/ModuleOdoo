# ModuleOdoo

# Odoo_module1.1
LO NECESARIO
1-Tener una maquina virtual con odoo
2-putty(en caso de trabajar con windows)
EMPEZAMOS
1-Lo primero que debemos hacer es iniciar nuestro odoo para ver que todo funciona correctamente
esto se hace desde la terminal de putty con los siguientes comandos:
cd /opt/odoo
y lanzamos nuestra base ./odoo.py
2-Una vez hecho esto vamos a nuestro explorador y ponemos la ip de nuestra maquina virtual con la terminacion :8069 si todo funciona podemos empezar ya con la instalacion de nuestros modulos
3-Abrimos el netbeans y creamos un proyecto php con remote server y lo configuramos todo correctamente, anteriormente siempre instalabamos el theme_default en este caso no instalaremos el openacademy que es con el q vamos a trabajar.
4-Una vez creado nuestro proyecto probamos que todo funciona correctamente desde el mismo proyecto podemos hacer un upload para ver si esta perfecto.
YA PODEMOS EMPEZAR CON EL CODIGO
Lo primero que debemos modificar son todos los siguientes archivos:
1-en openrp.py sustituimos todo el contenido por el que viene a continuacion:
# -*- coding: utf-8 -*-
{
    'name': "Open Academy",

    'summary': """Manage trainings""",

    'description': """
        Open Academy module for managing trainings:
            - training courses
            - training sessions
            - attendees registration
    """,

    'author': "My Company",
    'website': "http://www.yourcompany.com",

    # Categories can be used to filter modules in modules listing
    # Check https://github.com/odoo/odoo/blob/master/openerp/addons/base/module/module_data.xml
    # for the full list
    'category': 'Test',
    'version': '0.1',

    # any module necessary for this one to work correctly
    'depends': ['base'],

    # always loaded
    'data': [
        # 'security/ir.model.access.csv',
        'templates.xml',
    ],
    # only loaded in demonstration mode
    'demo': [
        'demo.xml',
    ],
}

2-openacademy/init.py
# -*- coding: utf-8 -*-
from . import controllers
from . import models

3-openacademy/controllers.py
# -*- coding: utf-8 -*-
from openerp import http

# class Openacademy(http.Controller):
#     @http.route('/openacademy/openacademy/', auth='public')
#     def index(self, **kw):
#         return "Hello, world"

#     @http.route('/openacademy/openacademy/objects/', auth='public')
#     def list(self, **kw):
#         return http.request.render('openacademy.listing', {
#             'root': '/openacademy/openacademy',
#             'objects': http.request.env['openacademy.openacademy'].search([]),
#         })

#     @http.route('/openacademy/openacademy/objects/<model("openacademy.openacademy"):obj>/', auth='public')
#     def object(self, obj, **kw):
#         return http.request.render('openacademy.object', {
#             'object': obj
#         })

3-demo.xml
<openerp>
    <data>
        <!--  -->
        <!--   <record id="object0" model="openacademy.openacademy"> -->
        <!--     <field name="name">Object 0</field> -->
        <!--   </record> -->
        <!--  -->
        <!--   <record id="object1" model="openacademy.openacademy"> -->
        <!--     <field name="name">Object 1</field> -->
        <!--   </record> -->
        <!--  -->
        <!--   <record id="object2" model="openacademy.openacademy"> -->
        <!--     <field name="name">Object 2</field> -->
        <!--   </record> -->
        <!--  -->
        <!--   <record id="object3" model="openacademy.openacademy"> -->
        <!--     <field name="name">Object 3</field> -->
        <!--   </record> -->
        <!--  -->
        <!--   <record id="object4" model="openacademy.openacademy"> -->
        <!--     <field name="name">Object 4</field> -->
        <!--   </record> -->
        <!--  -->
    </data>
</openerp>

4-models.py
# -*- coding: utf-8 -*-

from openerp import models, fields, api

# class openacademy(models.Model):
#     _name = 'openacademy.openacademy'

#     name = fields.Char()

5-templates.xml:

<openerp>
    <data>
        <!-- <template id="listing"> -->
        <!--   <ul> -->
        <!--     <li t-foreach="objects" t-as="object"> -->
        <!--       <a t-attf-href="{{ root }}/objects/{{ object.id }}"> -->
        <!--         <t t-esc="object.display_name"/> -->
        <!--       </a> -->
        <!--     </li> -->
        <!--   </ul> -->
        <!-- </template> -->
        <!-- <template id="object"> -->
        <!--   <h1><t t-esc="object.display_name"/></h1> -->
        <!--   <dl> -->
        <!--     <t t-foreach="object._fields" t-as="field"> -->
        <!--       <dt><t t-esc="field"/></dt> -->
        <!--       <dd><t t-esc="object[field]"/></dd> -->
        <!--     </t> -->
        <!--   </dl> -->
        <!-- </template> -->
    </data>
</openerp>

Ahora tenemos lo escencial para poder trabajar e ir agragandole funcionalidades

Lo primero que vamos a añadir va a ser una pestaña llamada Courses
1-Modificamos el model.py añadiendo el siguiente codigo:

class Course(models.Model):
    _name = 'openacademy.course'

    name = fields.Char(string="Title", required=True)
    description = fields.Text()
Tambien tenemos que modificar el archivo xml demo.xml, añadiendo el siguiente <record>:

<record model="openacademy.course" id="course0">
            <field name="name">Course 0</field>
            <field name="description">Course 0's description

Can have multiple lines
            </field>
        </record>
        <record model="openacademy.course" id="course1">
            <field name="name">Course 1</field>
            <!-- no description for this one -->
        </record>
        <record model="openacademy.course" id="course2">
            <field name="name">Course 2</field>
            <field name="description">Course 2's description</field>
        </record>
        
        Una vez creado necesitamos poder acceder a ella por lo tanto vamos a añadir un boton llamado courses que nos dara acceso a el
        1-Para hacer esto debemos añadir openrp.py en data el archivo de referencia(openacademy.xml)
        2-Modificamos el openacademy.xml:
        <?xml version="1.0" encoding="UTF-8"?>
<openerp>
    <data>
        <!-- window action -->
        <!--
            The following tag is an action definition for a "window action",
            that is an action opening a view or a set of views
        -->
        <record model="ir.actions.act_window" id="course_list_action">
            <field name="name">Courses</field>
            <field name="res_model">openacademy.course</field>
            <field name="view_type">form</field>
            <field name="view_mode">tree,form</field>
            <field name="help" type="html">
                <p class="oe_view_nocontent_create">Create the first course
                </p>
            </field>
        </record>

        <!-- top level menu: no parent -->
        <menuitem id="main_openacademy_menu" name="Open Academy"/>
        <!-- A first level in the left side menu is needed
             before using action= attribute -->
        <menuitem id="openacademy_menu" name="Open Academy"
                  parent="main_openacademy_menu"/>
        <!-- the following menuitem should appear *after*
             its parent openacademy_menu and *after* its
             action course_list_action -->
        <menuitem id="courses_menu" name="Courses" parent="openacademy_menu"
                  action="course_list_action"/>
        <!-- Full id location:
             action="openacademy.course_list_action"
             It is not required when it is the same module -->
    </data>
</openerp>

3-Anadimos un record para ver cada course con su descripcion:
<record model="ir.ui.view" id="course_form_view">
            <field name="name">course.form</field>
            <field name="model">openacademy.course</field>
            <field name="arch" type="xml">
                <form string="Course Form">
                    <sheet>
                        <group>
                            <field name="name"/>
                            <field name="description"/>
                        </group>
                    </sheet>
                </form>
            </field>
        </record>
        
 4-Añadimos un notebook para crear paginas dentro de cada course el siguiente codigo va despues del field del anterior:
 <notebook>
                            <page string="Description">
                                <field name="description"/>
                            </page>
                            <page string="About">
                                This is an example of notebooks
                            </page>
                        </notebook>
 
  5-Añadimos otro record al archivo openacademy.xml para poder buscar por titulo y descripcion de cada course:
  <record model="ir.ui.view" id="course_search_view">
            <field name="name">course.search</field>
            <field name="model">openacademy.course</field>
            <field name="arch" type="xml">
                <search>
                    <field name="name"/>
                    <field name="description"/>
                </search>
            </field>
        </record>
Ya con este ultimo paso hemos acabado con los courses.

Ahora vamos a añadir Sessions
1-Lo primero es añadir en models la siguiente clase:
class Session(models.Model):
    _name = 'openacademy.session'

    name = fields.Char(required=True)
    start_date = fields.Date()
    duration = fields.Float(digits=(6, 2), help="Duration in days")
    seats = fields.Integer(string="Number of seats")

2-Añadimos un nuevo record en openacademy.xml
<!-- session form view -->
        <record model="ir.ui.view" id="session_form_view">
            <field name="name">session.form</field>
            <field name="model">openacademy.session</field>
            <field name="arch" type="xml">
                <form string="Session Form">
                    <sheet>
                        <group>
                            <field name="name"/>
                            <field name="start_date"/>
                            <field name="duration"/>
                            <field name="seats"/>
                        </group>
                    </sheet>
                </form>
            </field>
        </record>

        <record model="ir.actions.act_window" id="session_list_action">
            <field name="name">Sessions</field>
            <field name="res_model">openacademy.session</field>
            <field name="view_type">form</field>
            <field name="view_mode">tree,form</field>
        </record>

        <menuitem id="session_menu" name="Sessions"
                  parent="openacademy_menu"
                  action="session_list_action"/>
        
Ahora vamos con las rlaciones one2many,many2one, y many2many2 entre los coureses los attendess y las sessions.
Many2One
1-Añadimos en models.py a la clase course:
responsible_id = fields.Many2one('res.users',
        ondelete='set null', string="Responsible", index=True)
2-Añadimos en models.py a ka clase session:
instructor_id = fields.Many2one('res.partner', string="Instructor")
    course_id = fields.Many2one('openacademy.course',
        ondelete='cascade', string="Course", required=True)
3-Dentro del group del course_form_view del openacademy.xml añadimos lo siguiente:
<field name="responsible_id"/>
4-dentro del openacademy.xml lo siguiente lo que hara es sobreescribir la lista de courses transformandola en una tabla con un elemento mas el responsable del curso:
<!-- override the automatically generated list view for courses -->
        <record model="ir.ui.view" id="course_tree_view">
            <field name="name">course.tree</field>
            <field name="model">openacademy.course</field>
            <field name="arch" type="xml">
                <tree string="Course Tree">
                    <field name="name"/>
                    <field name="responsible_id"/>
                </tree>
            </field>
        </record>
  Estos grupos se ñaden al session_form_view lo cual nos permitira al crear las sesion indicar a que curso pertenece y ponerle nombre y un instructor, por otra parte nos permite ponerle fecha,duracion y numeros de asientos disponibles en la sesion:
  <group string="General">
                                <field name="course_id"/>
                                <field name="name"/>
                                <field name="instructor_id"/>
                            </group>
                            <group string="Schedule">
                                <field name="start_date"/>
                                <field name="duration"/>
                                <field name="seats"/>
                            </group>
    El siguiente record se añade como los anteriores nos va a generar la tabla con las sesions creadas,nombre y curso al que pertenece:
    <!-- session tree/list view -->
        <record model="ir.ui.view" id="session_tree_view">
            <field name="name">session.tree</field>
            <field name="model">openacademy.session</field>
            <field name="arch" type="xml">
                <tree string="Session Tree">
                    <field name="name"/>
                    <field name="course_id"/>
                </tree>
            </field>
        </record>
        One2many:

añadimos a la clase course de models.py lo siguiente:

session_ids = fields.One2many(
        'openacademy.session', 'course_id', string="Sessions")

en el openacademy.xml dentro del notebook de course_form_view añadimos esta página, podemo borar una de muestra que se llama about

<page string="Sessions">
                                <field name="session_ids">
                                    <tree string="Registered sessions">
                                        <field name="name"/>
                                        <field name="instructor_id"/>
                                    </tree>
                                </field>

Many2many:

en models.py añadimos a la clase session:

 attendee_ids = fields.Many2many('res.partner', string="Attendees")

en openacademy.xml añadimos un label y un field a session_form_view justo debajo del group antes de cerrar sheet:

<label for="attendee_ids"/>
<field name="attendee_ids"/>

Una vez hecho esto ya podremos crear cursos y sessiones y ver como se relacionan unos con otros.

Ahora vamos a crear contacts y tags
Ahora vamos a crear una lista de contactos y una de tag asociados a ellos para asi a poder añadir esos contactos a las sesiones o como responsable de un curso o instructor de una sesion. Para ello crearemos un partner.py y un partner.xml ambos en la carpeta openacademy

importaremos en init.py partner para que se lance:

from . import partner

y en partner.py crearemos la clase partner asi como una relacion many2many con las sesiones

# -*- coding: utf-8 -*-
from openerp import fields, models

class Partner(models.Model):
    _inherit = 'res.partner'

    # Add a new column to the res.partner model, by default partners are not
    # instructors
    instructor = fields.Boolean("Instructor", default=False)

    session_ids = fields.Many2many('openacademy.session',
        string="Attended Sessions", readonly=True)

en el partner.xml añadimos todo eso:

<?xml version="1.0" encoding="UTF-8"?>
 <openerp>
    <data>
        <!-- Add instructor field to existing view -->
        <record model="ir.ui.view" id="partner_instructor_form_view">
            <field name="name">partner.instructor</field>
            <field name="model">res.partner</field>
            <field name="inherit_id" ref="base.view_partner_form"/>
            <field name="arch" type="xml">
                <notebook position="inside">
                    <page string="Sessions">
                        <group>
                            <field name="instructor"/>
                            <field name="session_ids"/>
                        </group>
                    </page>
                </notebook>
            </field>
        </record>

        <record model="ir.actions.act_window" id="contact_list_action">
            <field name="name">Contacts</field>
            <field name="res_model">res.partner</field>
            <field name="view_mode">tree,form</field>
        </record>
        <menuitem id="configuration_menu" name="Configuration"
                  parent="main_openacademy_menu"/>
        <menuitem id="contact_menu" name="Contacts"
                  parent="configuration_menu"
                  action="contact_list_action"/>
    </data>
</openerp>

bien ahora y ya deberiamos ser capaces una vez subido el codigo al servidor y actualizado el modulo de ver una seccion de configuracíon nueva con dos pestañas accesible.

Ahora vamos a crear dos tags especificos y vamos a hacer que solo los contactos que tengan estos tags sean seleccionable a la hora de elegir un intructor para una sesion.

en models.py y e la clase session modificamos el instructor_id:

instructor_id = fields.Many2one('res.partner', string="Instructor",
        domain=['|', ('instructor', '=', True),
                     ('category_id.name', 'ilike', "Teacher")])

esto lo añadimos dentro de partner.xml antes de cerrar data, esto nos da una lista de tags por defecto y los dos tags que nosotros creamos.

<record model="ir.actions.act_window" id="contact_cat_list_action">
            <field name="name">Contact Tags</field>
            <field name="res_model">res.partner.category</field>
            <field name="view_mode">tree,form</field>
        </record>
        <menuitem id="contact_cat_menu" name="Contact Tags"
                  parent="configuration_menu"
                  action="contact_cat_list_action"/>

        <record model="res.partner.category" id="teacher1">
            <field name="name">Teacher / Level 1</field>
        </record>
        <record model="res.partner.category" id="teacher2">
            <field name="name">Teacher / Level 2</field>
        </record>

Ahora a la hora de añadir un instructor a una session solo podremos añadir los contactos con estos dos tags específicos
Modificaciones En Session

Vamos a añadir tanto a la hora de crear una session como una vez creada una barra que nos indique el aforo de la sesion.

en models.py añadimos lo siguiente a la clase session:

taken_seats = fields.Float(string="Taken seats", compute='_taken_seats')

    @api.depends('seats', 'attendee_ids')
    def _taken_seats(self):
        for r in self:
            if not r.seats:
                r.taken_seats = 0.0
            else:
                r.taken_seats = 100.0 * len(r.attendee_ids) / r.seats

y en openacademy.xml en session_form_view lo siguiente en group "shedule" al final antes de cerrarlo:

<field name="taken_seats" widget="progressbar"/>

en session_tree_view en el tree antes de cerrarlo esto:

<field name="taken_seats" widget="progressbar"/>

Vamos a hacer que por defecto al crear una session nos ponga la fecha actual con la opcion de cambiarlo si queremos.

en models.py dentro de la clase session cambiamos el star_date:

start_date = fields.Date(default=fields.Date.today)

y añadimos al final de la clase session, asi la sesion esta activa por defecto:

active = fields.Boolean(default=True)

en el openacademy.xml en el group "general" de session_form_view añadimos esto antes de cerralo:

<field name="active"/>

Ya tenemos la sesion activa por defecto al crearla y la fecha actual seleccionada:
Warnnings Emergentes

Vamos a crear unas ventanas de aviso emergentes parecidas a las toast de android que saltaran si determinadas variable no son correcta, por ejemplo si hay mas asistentes que asisentos en la session o si introducimos un numero negativo de asientos.

unicamente añadimos en models.py fuera de las clases lo siguiente:

@api.onchange('seats', 'attendee_ids')
    def _verify_valid_seats(self):
        if self.seats < 0:
            return {
                'warning': {
                    'title': "Incorrect 'seats' value",
                    'message': "The number of available seats may not be negative",
                },
            }
        if self.seats < len(self.attendee_ids):
            return {
                'warning': {
                    'title': "Too many attendees",
                    'message': "Increase seats or remove excess attendees",
                },
            }

Constrains

Ahora usaremos otra forma de controlar los datos que vamos introduciendo, por ejemplo que no sea posible que un instructor asista a su propia sesion o que dos cursos se llamen igual. Estos no aparecen asta que intentamos guardar la sesion o el courso mientras que en el caso anterior son instantanes all cambiar de campo de escritura.

Para evitar que un instructor asista a su sesion:

modificamos en modesl.py la linea de importacion:

from openerp import models, fields, api, exceptions

y añadimos fuera de las clases otro metodo o funcion predefinida con la que controlar esto:

@api.constrains('instructor_id', 'attendee_ids')
    def _check_instructor_not_in_attendees(self):
        for r in self:
            if r.instructor_id and r.instructor_id in r.attendee_ids:
                raise exceptions.ValidationError("A session's instructor can't be an attendee")

Para comprobar si el nombre de la descripcion y el curso son diferentes, nos saltar un aviso al guardar si son iguales y tambien hacemos que el nombre sea unico.

añadimos en models.py dentro de la clase course al final:

sql_constraints = [
        ('name_description_check',
         'CHECK(name != description)',
         "The title of the course should not be the description"),

        ('name_unique',
         'UNIQUE(name)',
         "The course title must be unique"),
    ]

Como hicimos que el nombre sea unico ahora al crea una copia de un curso no nos dejaran hacerlo. Vamos permitir que nos deje duplicar u curso pero cambiado el nombre a "Copy of" [nombre original], asi si podremos hacer duplicados de cursos.

en models.py añadimos en la clase course antes del _sql_constraints:

@api.multi
    def copy(self, default=None):
        default = dict(default or {})

        copied_count = self.search_count(
            [('name', '=like', u"Copy of {}%".format(self.name))])
        if not copied_count:
            new_name = u"Copy of {}".format(self.name)
        else:
            new_name = u"Copy of {} ({})".format(self.name, copied_count)

        default['name'] = new_name
        return super(Course, self).copy(default)

Advanced Views

Ya hemos terminado con lo básico ahora vamos a ver como implementar algunas herramientas que nos faciliten el uso y visualizacion de todos los datos:

    Tree views: vamos a establecer que las sessiones tengan un color diferente segun su duracion, azul<5>negro<15>rojo, en dias.
    Calendars: un calendario en el que ver de manera organizada las sesiones.
    Search views: añadiremos un tag a la barra de busqueda de los cursos para que busque en mis cursos por defecto, asi un usuario solo verá en principio los cursos en los que participa de alguna forma.
    Gantt: implementaremos este tipo de diagrama.
    Graph views: implementaremos graficos para analizar algunos datos de los cursos y sesiones.
    Kanban: con esto agruparemos las sesiones por cada curso pudiendo minimizar maximizar o desplazar un curso par ordenarlo a nuestro gusto.

TREE VIEWS:

unicmente modificaremos el session_tree_view:

<tree string="Session Tree" decoration-info="duration&lt;5" decoration-danger="duration&gt;15">

añadiremos un field al tree antes de la barra de progreso.

<field name="duration" invisible="1"/>

CALENDARS:

es un poco mas complicado de implemetar, al igaul que tenemos un boton para abrir el detalle de una session, tendremos ahora uno para abrir el calendario.

en models.py teenmos que importar lo sigueinte:

from datetime import timedelta

añadir esto en la clase session antes de @api.depends:

end_date = fields.Date(string="End Date", store=True,
        compute='_get_end_date', inverse='_set_end_date')

y esto en la misma clase antes de @api.constraints:

@api.depends('start_date', 'duration')
    def _get_end_date(self):
        for r in self:
            if not (r.start_date and r.duration):
                r.end_date = r.start_date
                continue

            # Add duration to start_date, but: Monday + 5 days = Saturday, so
            # subtract one second to get on Friday instead
            start = fields.Datetime.from_string(r.start_date)
            duration = timedelta(days=r.duration, seconds=-1)
            r.end_date = start + duration

    def _set_end_date(self):
        for r in self:
            if not (r.start_date and r.end_date):
                continue

            # Compute the difference between dates, but: Friday - Monday = 4 days,
            # so add one day to get 5 days instead
            start_date = fields.Datetime.from_string(r.start_date)
            end_date = fields.Datetime.from_string(r.end_date)
            r.duration = (end_date - start_date).days + 1

en oepenacademy.xml añadimos es record antes del session_list_action:

<!-- calendar view -->
        <record model="ir.ui.view" id="session_calendar_view">
            <field name="name">session.calendar</field>
            <field name="model">openacademy.session</field>
            <field name="arch" type="xml">
                <calendar string="Session Calendar" date_start="start_date"
                          date_stop="end_date"
                          color="instructor_id">
                    <field name="name"/>
                </calendar>
            </field>
        </record>

y en el session_list_action modificamos es este field:

<field name="view_mode">tree,form,calendar</field>

SEACH VIEWS

unicamente tabajamos en openacademy.xml, dentro de course_search_view añadimos dentre de search:

<filter name="my_courses" string="My Courses"
                            domain="[('responsible_id', '=', uid)]"/>
                    <group string="Group By">
                        <filter name="by_responsible" string="Responsible"
                                context="{'group_by': 'responsible_id'}"/>
                    </group>

en el course_list_action un field mas antes del ultimo:

<field name="context" eval="{'search_default_my_courses': 1}"/>

GANTT:

en models.py añadimos dentro de la clase session y despues de end_date:

hours = fields.Float(string="Duration in hours",
                         compute='_get_hours', inverse='_set_hours')

en la misma clase antes de @api.constrains:

@api.depends('duration')
    def _get_hours(self):
        for r in self:
            r.hours = r.duration * 24

    def _set_hours(self):
        for r in self:
            r.duration = r.hours / 24

en openacademy.xml añadimos un record antes del session_list_action:

<record model="ir.ui.view" id="session_gantt_view">
            <field name="name">session.gantt</field>
            <field name="model">openacademy.session</field>
            <field name="arch" type="xml">
                <gantt string="Session Gantt" color="course_id"
                       date_start="start_date" date_delay="hours"
                       default_group_by='instructor_id'>
                    <field name="name"/>
                </gantt>
            </field>
        </record>

y modificamos el ultimo field del mismo list_action:

<field name="view_mode">tree,form,calendar,gantt</field>

GRAPH VIEWS

en models.py añadimos en lo clase session despues del hours añadido en pasos anteriores:

attendees_count = fields.Integer(
        string="Attendees count", compute='_get_attendees_count', store=True)

tambien antes de @api.constrains lo siguiente:

@api.depends('attendee_ids')
    def _get_attendees_count(self):
        for r in self:
            r.attendees_count = len(r.attendee_ids)

en openacademy.py añadimos un record mas antes de session_list_action:

<record model="ir.ui.view" id="openacademy_session_graph_view">
            <field name="name">openacademy.session.graph</field>
            <field name="model">openacademy.session</field>
            <field name="arch" type="xml">
                <graph string="Participations by Courses">
                    <field name="course_id"/>
                    <field name="attendees_count" type="measure"/>
                </graph>
            </field>
        </record>

modificamo el ultimo field del mismo list_action:

 <field name="view_mode">tree,form,calendar,gantt,graph</field>

KANBAN:

en models.py añadimos una linea debajo de active en la clase session:

color = fields.Integer()

en open academy.xml añadimos el record correspondiente:

<record model="ir.ui.view" id="view_openacad_session_kanban">
            <field name="name">openacad.session.kanban</field>
            <field name="model">openacademy.session</field>
            <field name="arch" type="xml">
                <kanban default_group_by="course_id">
                    <field name="color"/>
                    <templates>
                        <t t-name="kanban-box">
                            <div
                                    t-attf-class="oe_kanban_color_{{kanban_getcolor(record.color.raw_value)}}
                                                  oe_kanban_global_click_edit oe_semantic_html_override
                                                  oe_kanban_card {{record.group_fancy==1 ? 'oe_kanban_card_fancy' : ''}}">
                                <div class="oe_dropdown_kanban">
                                    <!-- dropdown menu -->
                                    <div class="oe_dropdown_toggle">
                                        <i class="fa fa-bars fa-lg"/>
                                        <ul class="oe_dropdown_menu">
                                            <li>
                                                <a type="delete">Delete</a>
                                            </li>
                                            <li>
                                                <ul class="oe_kanban_colorpicker"
                                                    data-field="color"/>
                                            </li>
                                        </ul>
                                    </div>
                                    <div class="oe_clear"></div>
                                </div>
                                <div t-attf-class="oe_kanban_content">
                                    <!-- title -->
                                    Session name:
                                    <field name="name"/>
                                    <br/>
                                    Start date:
                                    <field name="start_date"/>
                                    <br/>
                                    duration:
                                    <field name="duration"/>
                                </div>
                            </div>
                        </t>
                    </templates>
                </kanban>
            </field>
        </record>

y modificamos el list_action como anteriormente:

<field name="view_mode">tree,form,calendar,gantt,graph,kanban</field>
