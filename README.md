package com.smarttask;

import javafx.application.Application; import javafx.scene.Scene; import javafx.scene.control.; import javafx.scene.layout.; import javafx.stage.Stage;

import java.util.*;

public class SmartTaskApp extends Application {

private final List<Task> tasks = new ArrayList<>();
private final VBox taskListBox = new VBox(10);

public static void main(String[] args) {
    launch(args);
}

@Override
public void start(Stage primaryStage) {
    primaryStage.setTitle("SmartTask - Productivity Manager");

    TextField taskInput = new TextField();
    taskInput.setPromptText("Enter a new task");

    Button addButton = new Button("Add Task");
    addButton.setOnAction(e -> {
        String text = taskInput.getText();
        if (!text.isEmpty()) {
            Task newTask = new Task(text);
            tasks.add(newTask);
            updateTaskList();
            taskInput.clear();
        }
    });

    VBox inputBox = new VBox(10, taskInput, addButton);
    inputBox.setStyle("-fx-padding: 20");

    ScrollPane scrollPane = new ScrollPane(taskListBox);
    scrollPane.setFitToWidth(true);

    VBox mainLayout = new VBox(20, inputBox, scrollPane);
    Scene scene = new Scene(mainLayout, 400, 500);

    primaryStage.setScene(scene);
    primaryStage.show();
}

private void updateTaskList() {
    taskListBox.getChildren().clear();
    for (Task task : tasks) {
        CheckBox checkBox = new CheckBox(task.getDescription());
        checkBox.setSelected(task.isCompleted());
        checkBox.setOnAction(e -> task.setCompleted(checkBox.isSelected()));
        taskListBox.getChildren().add(checkBox);
    }
}

}

class Task { private final String description; private boolean completed;

public Task(String description) {
    this.description = description;
    this.completed = false;
}

public String getDescription() {
    return description;
}

public boolean isCompleted() {
    return completed;
}

public void setCompleted(boolean completed) {
    this.completed = completed;
}

}

