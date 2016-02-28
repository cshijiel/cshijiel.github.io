title: "SpringMVC URL传参方式"
date: 2015-04-15 11:04:43
tags:
- SpringMVC
categories: 知识记录
---

1. ModalAndView
    ```Java
    @Controller
    public class HelloWorldController {
    
        @RequestMapping("/helloWorld")
        public ModelAndView helloWorld() {
            ModelAndView mav = new ModelAndView();
            mav.setViewName("helloWorld");//viewName 视图名称
            mav.addObject("message", "Hello World!");// 往视图传递对象
            return mav;
        }
    }
    ```
2. 在Class级别这是`@RequestMapping`
    ```
    @Controller
    @RequestMapping("/appointments")
    public class AppointmentsController {
    
        private final AppointmentBook appointmentBook;
        
        @Autowired
        public AppointmentsController(AppointmentBook appointmentBook) {
            this.appointmentBook = appointmentBook;
        }
    
        @RequestMapping(method = RequestMethod.GET)
        public Map<String, Appointment> get() {
            return appointmentBook.getAppointmentsForToday();
        }
    
        @RequestMapping(value="/{day}", method = RequestMethod.GET)
        public Map<String, Appointment> getForDay(@PathVariable @DateTimeFormat(iso=ISO.DATE) Date day, Model model) {
            return appointmentBook.getAppointmentsForDay(day);
        }
    
        @RequestMapping(value="/new", method = RequestMethod.GET)
        public AppointmentForm getNewForm() {
            return new AppointmentForm();
        }
    
        @RequestMapping(method = RequestMethod.POST)
        public String add(@Valid AppointmentForm appointment, BindingResult result) {
            if (result.hasErrors()) {
                return "appointments/new";
            }
            appointmentBook.addAppointment(appointment);
            return "redirect:/appointments";
        }
    }
    ```
3. @PathVirable
    ```
    @RequestMapping(value="/owners/{ownerId}", method=RequestMethod.GET)
    public String findOwner(@PathVariable String ownerId, Model model) {
      Owner owner = ownerService.findOwner(ownerId);  
      model.addAttribute("owner", owner);  
      return "displayOwner"; 
    }
    
    
    @RequestMapping(value="/owners/{ownerId}", method=RequestMethod.GET)
    public String findOwner(@PathVariable("ownerId") String ownerId, Model model) {
      // implementation omitted
    }
    ```

>待完善...