CodeIgniter YAML Forge
======================
This library aims to generate database schemas from a basic YAML defined schema. I find YAML notation a lot easier to read/and write than verbose SQL scripts or $this->dbforge->add_field( ... ) and I'm hoping this abtraction will save me some time and some keystrokes. It utilizes the Symfony YAML libraries and the dbforge library found in CodeIgniter for the schema generation.

This is just a first draft of the idea which I cooked up today... I've designed it with my own conventions and the fact that I use WanWizard's DataMapper ORM should be kept in mind.

Currently the schema supports:
* Creates tables
* Creates fields
* Automagically adds relationship fields and tables (based on the DataMapper ORM convention)

Documentation
-------------

This example controller shows the basic usage:

    class Test_yaml_forge extends CI_Controller {
    
        public function index()
        {
            $this->load->spark('yaml_forge/0.0.1');
        
            $this->yaml_forge->set_debug(TRUE);       // debug: verbose output, FALSE by default
            $this->yaml_forge->set_auto_id(TRUE);     // auto_id: adds an 'id' field to each table, TRUE by default
            $this->yaml_forge->set_drop_tables(TRUE); // drop_tables: drops a table before creating, FALSE by default

            $this->yaml_forge->generate( APPPATH . '../sparks/yaml_forge/0.0.1/test/test_schema.yaml' );
        }
    }

This is how the YAML should look:

    "users":
      first_name: 
        type: varchar
        constraint: 255
      last_name:
        type: varchar
        constraint: 255

    "categories":
      has_many: [posts]
      name:
        type: varchar
        constraint: 255

    "posts":
      has_many: [categories]
      has_one: [users]
      title:
        type: varchar
        constraint: 255
      body:
        type: text
      date_created:
        type: datetime
      date_updated: "TIMESTAMP ON UPDATE CURRENT_TIMESTAMP DEFAULT CURRENT_TIMESTAMP"


Contributing
------------
Drop me a line if you use this library or have any thoughts. Contribution would also be great.