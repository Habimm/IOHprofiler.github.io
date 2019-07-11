---
layout: page
title: CreatingSuites
parent: IOHexperimenter
nav_order: 3
permalink: /IOHexperimenter/CreatingSuites/
--- 

Creating Suites
==================================
[IOHprofiler_suite]() is the base `class` of suites of __IOHexperimenter__. The property variables of problems include:
* `problem_id`, a vector of id which problems are to be tested.
* `instance_id`, a vector of instance id of problems. intanced id sets the transformation on the problem. The original problem is with instance_id 1, <i>scale</i> and <i>shift</i> are applied on objectives for instance_id in [2,100], <i>XOR</i> will be applied on variables for instance_id in [2,50], and <i>sigma</i> function is applied on variables for instance_id in [51,100].
* `dimension`, a vector of dimension of problems.
* `number_of_problems`
* `number_of_instances`
* `number_of_dimensions`


And some functions for experiments are supplied:
* <i>get_next_problem</i>, return a shared point of problems of the suite in order.
* <i>addCSVLogger(logger)</i>, assigns a __IOHprofiler_csv_logger__ class to the suite.
* <i>IOHprofiler_set_suite_problem_id(problem_id)</i>
* <i>IOHprofiler_set_suite_instance_id(instance_id)</i>
* <i>IOHprofiler_set_suite_dimension(dimension)</i>
* <i>mapIDTOName</i>, is to match problem id and name. 

__IOHexperimenter__ provides a __PBO_suite__ for pseudo Boolean problems, but it is also easy to add your own suite. Creating a suite is to register problems in the suite and assign id to problems.

Taking the implementation of __PBO_suite__ as an instance, <i>construct functions</i> are as below. In the construct functions, the range of allowed `problem_id`, `instance_id` and `dimension` should be identified. In addition, <i>registerProblem()</i> must be called when a suite is constructed.
```cpp
PBO_suite() {
  std::vector<int> problem_id;
  std::vector<int> instance_id;
  std::vector<int> dimension;
  for (int i = 0; i < 23; ++i) {
    problem_id.push_back(i+1);
  }
  for (int i = 0; i < 1; ++i) {
    instance_id.push_back(i+1);
  }
  dimension.push_back(100);
  
  IOHprofiler_set_suite_problem_id(problem_id);
  IOHprofiler_set_suite_instance_id(instance_id);
  IOHprofiler_set_suite_dimension(dimension);
  IOHprofiler_set_suite_name("PBO");
  registerProblem();
};

PBO_suite(std::vector<int> problem_id, std::vector<int> instance_id, std::vector<int> dimension) {
  for (int i = 0; i < problem_id.size(); ++i) {
    if (problem_id[i] < 0 || problem_id[i] > 23) {
      IOH_error("problem_id " + std::to_string(problem_id[i]) + " is not in PBO_suite");
    }
  }
  
  for (int i = 0; i < instance_id.size(); ++i) {
    if (instance_id[i] < 0 || instance_id[i] > 100) {
      IOH_error("instance_id " + std::to_string(instance_id[i]) + " is not in PBO_suite");
    }
  }

  for (int i = 0; i < dimension.size(); ++i) {
    if (dimension[i] < 0 || dimension[i] > 20000) {
      IOH_error("dimension " + std::to_string(dimension[i]) + " is not in PBO_suite");
    }
  }

  IOHprofiler_set_suite_problem_id(problem_id);
  IOHprofiler_set_suite_instance_id(instance_id);
  IOHprofiler_set_suite_dimension(dimension);
  IOHprofiler_set_suite_name("PBO");
  registerProblem();
}
```

<i>registerProblem()</i> is a virtual function of the base `IOHprofiler_suite` class. When you create a suite, it __must__ be implemented. In the function, problems included in the suite need to be registered, and problem id and name should be mapped. The following is the <i>registerProblem()</i> function of __PBO_suite__.
```cpp
  registerInFactory<IOHprofiler_problem<int>,OneMax> regOneMax("OneMax");
  registerInFactory<IOHprofiler_problem<int>,OneMax_Dummy1> regOneMax_Dummy1("OneMax_Dummy1");
  registerInFactory<IOHprofiler_problem<int>,OneMax_Dummy2> regOneMax_Dummy2("OneMax_Dummy2");
  registerInFactory<IOHprofiler_problem<int>,OneMax_Epistasis> regOneMax_Epistasis("OneMax_Epistasis");
  registerInFactory<IOHprofiler_problem<int>,OneMax_Neutrality> regOneMax_Neutrality("OneMax_Neutrality");
  registerInFactory<IOHprofiler_problem<int>,OneMax_Ruggedness1> regOneMax_Ruggedness1("OneMax_Ruggedness1");
  registerInFactory<IOHprofiler_problem<int>,OneMax_Ruggedness2> regOneMax_Ruggedness2("OneMax_Ruggedness2");
  registerInFactory<IOHprofiler_problem<int>,OneMax_Ruggedness3> regOneMax_Ruggedness3("OneMax_Ruggedness3");

  registerInFactory<IOHprofiler_problem<int>,LeadingOnes> regLeadingOnes("LeadingOnes");
  registerInFactory<IOHprofiler_problem<int>,LeadingOnes_Dummy1> regLeadingOnes_Dummy1("LeadingOnes_Dummy1");
  registerInFactory<IOHprofiler_problem<int>,LeadingOnes_Dummy2> regLeadingOnes_Dummy2("LeadingOnes_Dummy2");
  registerInFactory<IOHprofiler_problem<int>,LeadingOnes_Epistasis> regLeadingOnes_Epistasis("LeadingOnes_Epistasis");
  registerInFactory<IOHprofiler_problem<int>,LeadingOnes_Neutrality> regLeadingOnes_Neutrality("LeadingOnes_Neutrality");
  registerInFactory<IOHprofiler_problem<int>,LeadingOnes_Ruggedness1> regLeadingOnes_Ruggedness1("LeadingOnes_Ruggedness1");
  registerInFactory<IOHprofiler_problem<int>,LeadingOnes_Ruggedness2> regLeadingOnes_Ruggedness2("LeadingOnes_Ruggedness2");
  registerInFactory<IOHprofiler_problem<int>,LeadingOnes_Ruggedness3> regLeadingOnes_Ruggedness3("LeadingOnes_Ruggedness3");
  
  registerInFactory<IOHprofiler_problem<int>,Linear> regLinear("Linear");
  registerInFactory<IOHprofiler_problem<int>,MIS> regMIS("MIS");
  registerInFactory<IOHprofiler_problem<int>,LABS> regLABS("LABS");
  registerInFactory<IOHprofiler_problem<int>,NQueens> regNQueens("NQueens");
  registerInFactory<IOHprofiler_problem<int>,Ising_1D> regIsing_1D("Ising_1D");
  registerInFactory<IOHprofiler_problem<int>,Ising_2D> regIsing_2D("Ising_2D");
  registerInFactory<IOHprofiler_problem<int>,Ising_Triangle> regIsing_Triangle("Ising_Triangle");

  mapIDTOName(1,"OneMax");
  mapIDTOName(2,"LeadingOnes");
  mapIDTOName(3,"Linear");
  mapIDTOName(4,"OneMax_Dummy1");
  mapIDTOName(5,"OneMax_Dummy2");
  mapIDTOName(6,"OneMax_Neutrality");
  mapIDTOName(7,"OneMax_Epistasis");
  mapIDTOName(8,"OneMax_Ruggedness1");
  mapIDTOName(9,"OneMax_Ruggedness2");
  mapIDTOName(10,"OneMax_Ruggedness3");
  mapIDTOName(11,"LeadingOnes_Dummy1");
  mapIDTOName(12,"LeadingOnes_Dummy2");
  mapIDTOName(13,"LeadingOnes_Neutrality");
  mapIDTOName(14,"LeadingOnes_Epistasis");
  mapIDTOName(15,"LeadingOnes_Ruggedness1");
  mapIDTOName(16,"LeadingOnes_Ruggedness2");
  mapIDTOName(17,"LeadingOnes_Ruggedness3");
  mapIDTOName(18,"LABS");
  mapIDTOName(22,"MIS");
  mapIDTOName(19,"Ising_1D");
  mapIDTOName(20,"Ising_2D");
  mapIDTOName(21,"Ising_Triangle");
  mapIDTOName(23,"NQueens");
```

If you want to register your problem by `suite_name`, please add following codes and modify names.
```cpp
static PBO_suite * createInstance() {
  return new PBO_suite();
};

static PBO_suite * createInstance(std::vector<int> problem_id, std::vector<int> instance_id, std::vector<int> dimension) {
  return new PBO_suite(problem_id, instance_id, dimension);
};
```
To register the suite, you can use the <i>geniricGenerator</i> in [IOHprofiler_class_generator](https://github.com/IOHprofiler/IOHexperimenter/blob/developing/src/Template/IOHprofiler_class_generator.hpp). For example, you can use the following statement to register and create __PBO_suite__ ,
```cpp
// Register
static registerInFactory<IOHprofiler_suite<int>,PBO_suite> regPBO("PBO");
// Create
std::shared_ptr<IOHprofiler_suite<InputType>> suite = genericGenerator<IOHprofiler_suite<int>>::instance().create("PBO");
);
```