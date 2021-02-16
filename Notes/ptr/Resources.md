# Resources

## Table of Contents

- [Resources](#resources)
  - [Table of Contents](#table-of-contents)
  - [Representing Resources](#representing-resources)
  - [Requesting a Resource](#requesting-a-resource)
  - [Releasing a Resource](#releasing-a-resource)
  - [The Scheduler](#the-scheduler)

## Representing Resources

The OS can represent various different resources such as hardware and software. Most can be represented by a **resource control block**, similar to PCBs

| RCB Field            | Explanation                                                                                                                                                                                                                                                                                            |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| resource_description | description of *r*'s properties and capabilities                                                                                                                                                                                                                                                       |
| state                | current availability of *r*. If *r* is a single-unit resource then the field indicates whether *r* is free or allocated to some process. If *r* has multiple identical units, such as a pool of identical buffers, then this field tracks how many units are currently free and how many are allocated |
| waiting_list         | when *r* is requested by a process and *r* is unavailable then that process's PCB is removed from the ready list and added to *r*'s waiting list                                                                                                                                                       |

## Requesting a Resource

A resource is **allocated** to a process when the process has access and is able to utilize it

A resource is **free** if the resource may be allocated to a requesting process

The **request resource function** allocates *r* to *p* or blocks *p* if *r* is currently unavailable

- if *r* is free, the state of *r* is changed to allocated, and a pointer to *r* is inserted into the list of *other_resources* of *p*
- if *r* is not free, *p* is blocked. *p*'s PCB is moved from the ready list to the *waiting_list* of *r*. As *p* is now blocked the scheduler must be called to select another process to run

```c
request(r) {
   if (r.state == free) {
      r.state = allocated
      Insert r into self.other_resources 
   }
   else {
      self.state = blocked
      Move self from RL to r.waiting_list 
      scheduler()
   }
}
```

## Releasing a Resource

The **release resource function** allocates the resource *r* to the next process on the waiting list. If the list is empty, *r* is marked free

```c
release(r) {
   Remove r from self.other_resources
   if (r.waiting_list == empty)
      r.state = free
   else
      Remove process q from the head of r.waiting_list
      Insert r into q.other_resources
      q.process_state = ready      
      Move q from r.waiting_list to RL
      scheduler()
}
```

## The Scheduler

The **scheduler function** determines which process should run next and starts that process. This function is called at the end of the *create*, *destroy*, *request*, and *release* management functions

```c
scheduler() {
   Find highest priority process q on RL
   if (p.priority < q.priority ) or (p.process_state == blocked)
      perform context switch from p to q
}
```
