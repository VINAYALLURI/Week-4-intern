import javafx.application.Application;
import javafx.collections.FXCollections;
import javafx.collections.ObservableList;
import javafx.geometry.Insets;
import javafx.scene.Scene;
import javafx.scene.control.*;
import javafx.scene.layout.GridPane;
import javafx.stage.Stage;

public class TodoApp extends Application {

    private ObservableList<String> tasks = FXCollections.observableArrayList();
    private TextField taskField = new TextField();
    private ListView<String> taskListView = new ListView<>(tasks);

    public static void main(String[] args) {
        launch(args);
    }

    @Override
    public void start(Stage primaryStage) {
        primaryStage.setTitle("To-Do List");

        GridPane gridPane = new GridPane();
        gridPane.setPadding(new Insets(10, 10, 10, 10));
        gridPane.setVgap(10);
        gridPane.setHgap(10);

        Label taskLabel = new Label("Task:");
        gridPane.add(taskLabel, 0, 0);

        gridPane.add(taskField, 1, 0);

        Button addButton = new Button("Add Task");
        addButton.setOnAction(e -> addTask());
        gridPane.add(addButton, 2, 0);

        taskListView.setPrefWidth(300);
        taskListView.setPrefHeight(200);
        gridPane.add(taskListView, 0, 1, 3, 1);

        Button removeButton = new Button("Remove Task");
        removeButton.setOnAction(e -> removeTask());
        gridPane.add(removeButton, 0, 2, 3, 1);

        primaryStage.setScene(new Scene(gridPane, 400, 300));
        primaryStage.show();
    }

    private void addTask() {
        String task = taskField.getText();
        if (!task.isEmpty()) {
            tasks.add(task);
            taskListView.setItems(tasks);
            taskField.clear();
        } else {
            Alert alert = new Alert(Alert.AlertType.WARNING);
            alert.setTitle("Warning");
            alert.setHeaderText(null);
            alert.setContentText("Task cannot be empty!");
            alert.showAndWait();
        }
    }

    private void removeTask() {
        int selectedTaskIndex = taskListView.getSelectionModel().getSelectedIndex();
        if (selectedTaskIndex != -1) {
            String task = tasks.remove(selectedTaskIndex);
            taskListView.setItems(tasks);
            Alert alert = new Alert(Alert.AlertType.INFORMATION);
            alert.setTitle("Task Removed");
            alert.setHeaderText(null);
            alert.setContentText("Task \"" + task + "\" has been removed.");
            alert.showAndWait();
        } else {
            Alert alert = new Alert(Alert.AlertType.WARNING);
            alert.setTitle("Warning");
            alert.setHeaderText(null);
            alert.setContentText("Please select a task to remove.");
            alert.showAndWait();
        }
    }
}
