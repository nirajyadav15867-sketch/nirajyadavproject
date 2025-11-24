# nirajyadavproject
this is my first repository
author ~ niraj yadav
# Student Management System (CSE) — Python

## Project Title
Student Management System (CSE) — CLI tool to manage student records and marks.

## Overview
A command-line Student Management System to add, update, delete, search, list and export student records. Records include personal details and marks for subjects; the system computes total, percentage and grade. Data is stored locally in `data/students.json`.

## Features
- Add / Update / Delete student records
- Store marks for subjects (subject names configurable)
- Compute total marks, percentage and grade automatically
- Search by roll or name (partial match)
- List all students (show percentage & grade)
- Export records to CSV (`exports/students.csv`)
- JSON persistence; simple tests included

## Technologies / Tools Used
- Python 3.8+
- Standard library: `json`, `csv`, `pathlib`, `dataclasses`
- `pytest` (for tests)

## Installation & Running
1. Clone repository:
```bash
git clone https://github.com/your-username/student-management-system-cse.git
cd student-management-system-cse
# src/student.py
from dataclasses import dataclass, asdict, field
from typing import Dict, List

@dataclass
class Student:
    roll: str
    name: str
    branch: str = "CSE"
    year: str = "1"
    contact: str = ""
    email: str = ""
    marks: Dict[str, int] = field(default_factory=dict)  # subject -> marks

    def to_dict(self):
        return asdict(self)

    @staticmethod
    def from_dict(d):
        return Student(
            roll=d.get("roll", ""),
            name=d.get("name", ""),
            branch=d.get("branch", "CSE"),
            year=d.get("year", "1"),
            contact=d.get("contact", ""),
            email=d.get("email", ""),
            marks=d.get("marks", {})
        )

    def total_marks(self) -> int:
        return sum(int(v) for v in self.marks.values()) if self.marks else 0

    def max_total(self, per_subject_max=100) -> int:
        return per_subject_max * len(self.marks) if self.marks else 0

    def percentage(self, per_subject_max=100) -> float:
        max_total = self.max_total(per_subject_max)
        if max_total == 0:
            return 0.0
        return (self.total_marks() / max_total) * 100.0

    def grade(self, per_subject_max=100) -> str:
        p = self.percentage(per_subject_max)
        if p >= 85:
            return "A"
        if p >= 70:
            return "B"
        if p >= 55:
            return "C"
        if p >= 40:
            return "D"
        return "F"
