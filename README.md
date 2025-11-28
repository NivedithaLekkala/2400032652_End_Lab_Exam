import React, { useState } from "react";
import { DragDropContext, Droppable, Draggable } from "react-beautiful-dnd";

function App() {
  const [courses, setCourses] = useState([
    "Data Structures",
    "Operating Systems",
    "Computer Networks",
    "Machine Learning",
    "Database Management Systems"
  ]);

  const handleOnDragEnd = (result) => {
    if (!result.destination) return;

    const items = Array.from(courses);
    const [reorderedItem] = items.splice(result.source.index, 1);
    items.splice(result.destination.index, 0, reorderedItem);

    setCourses(items);
  };

  return (
    <div style={{ width: "400px", margin: "40px auto", textAlign: "center" }}>
      <h2>Course List</h2>

      <DragDropContext onDragEnd={handleOnDragEnd}>
        <Droppable droppableId="courses">
          {(provided) => (
            <ul
              className="courses"
              {...provided.droppableProps}
              ref={provided.innerRef}
              style={{ padding: 0, listStyle: "none" }}>
              {courses.map((course, index) => (
                <Draggable key={course} draggableId={course} index={index}>
                  {(provided) => (
                    <li
                      {...provided.draggableProps}
                      {...provided.dragHandleProps}
                      ref={provided.innerRef}
                      style={{
                        padding: "12px",
                        margin: "8px 0",
                        background: "#e3e3e3",
                        borderRadius: "6px",
                        ...provided.draggableProps.style
                      }}
                    >
                      {course}
                    </li>
                  )}
                </Draggable>
              ))}
              {provided.placeholder}
            </ul>
          )}
        </Droppable>
      </DragDropContext>

      <h3>Updated Order:</h3>
      <p>{courses.join(" , ")}</p>
    </div>
  );
}

export default App;
