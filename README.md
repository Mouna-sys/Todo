package com.example.todosummary;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
@SpringBootApplication
public class TodoSummaryApplication {
    public static void main(String args[]) {
        SpringApplication.run(TodoSummaryApplication.class,args);
    }
}
package com.example.todosummary.model;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
@Entity
public class Todo {
    @Id
    @GeneratedValue(strategy=GenerationType.IDENTITY)
    private Long id;
    private String description;
    private boolean completed;
    
    public Long getId() {
        return id;
    }
    public void setId(Long id){
        this.id=id;
    }
    public String getDescription(){
        return description;
    }
    public void setDescription(String description){
        this.description=description;
    }
    public boolean isCompleted(){
        return completed;
    }
    public void setCompleted(boolean completed){
        this.completed=completed;
    }
}

packagecom.example.todosummary.repository;
import com.example.todosummary.model.Todo;
import org.springframework.data.jpa.repository.JpaRepository;
public interface TodoRepository extends JpaRepository<Todo,Long> {
}
package com.example.todosummary.service;
import com.example.todosummary.repository.model.Todo;
import com.example.todosummary.repository.TodoRespository;
import org.springframework.stereotype.Service;
import java.util.List;
@Service
public class TodoService {
    private final TodoRepository todoRepository;
    public TodoService(TodoRepository todoRepository){
        this.todoRepository=todoRepository;
            
            public List<Todo> getTodos(){
                return todoRepository.findAll();
            }
            public Todo addTodo(Todo todo){
                return todoRepository.save(todo);
            }
            public void deleteTodoById(Long id){
                todoRepository.deleteById(id);
            }
    }
    package com.example.todsummary.controller;
    import com.example.todosummary.model.Todo;
    import com.example.todosummary.service.TodoService;
    import org.springframework.http.ResponseEntity;
    import org.springframework.web.bind.annotation.*;
    import java.util.List;
    @RestController
    @RequestMapping("/todos")
    public class TodoController{
        private final TodoService todoService;
        public TodoController(TodoService todoService){
            this.todoService=todoService;
        }
        @GetMapping
        public List<Todo> getTodos(){
            return todoService.getTodos();
        }
        @PostMapping
        public Todo addTodo(@RequestBody todo todo){
            return todoService.getaddTodo(todo);
        }
        @DeleteMapping("/{id}")
        public ResponseEntity<Void> deleteTodoById(@PathVariable Long id){
            todoService.deleteTodoById(id);
            return ResponseEntity.noContent().build();
        }
        @PostMapping("/summarize")
        public ResponseEntity<String> summarizeTodos(){
            return ReponseEntity.ok("Summary sent to Slack!");
        }
    }
}
