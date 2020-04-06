---
title: mysql example models (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'models'


Modules used in program: 
* `import datetime`

## python models

Python mysql example: models

```python
from app import app, db
from flask_user import UserMixin, UserManager, SQLAlchemyAdapter
from flask_user.forms import RegisterForm
from passlib.apps import mysql_context
# import forms
import datetime


class User(db.Model, UserMixin):
  id = db.Column(db.Integer, primary_key=True)
  username = db.Column(db.String(50), nullable=False, unique=True)
  password = db.Column(db.String(255), nullable=False, default='')
  reset_password_token = db.Column(db.String(100), nullable=False, default='')

  # User Email information
  email = db.Column(db.String(255), nullable=False, unique=True)
  confirmed_at = db.Column(db.DateTime())

  # User information
  is_enabled = db.Column(db.Boolean(), nullable=False, default=False)
  first_name = db.Column(db.String(50), nullable=False, default='')
  last_name = db.Column(db.String(50), nullable=False, default='')

  roles = db.relationship('Role', secondary="user_roles", backref=db.backref('users', lazy='dynamic'))
  school_joins = db.relationship('SchoolCoordinatorJoin', backref='coordinator', lazy='dynamic')

  def is_active(self):
    return self.is_enabled

  def get_full_name(self):
    return "%s %s" % (self.first_name, self.last_name)

  def is_coordinator(self):
    role_ids = [user_role.role_id for user_role in db.session.query(UserRoles).filter_by(user_id=self.id).all()]
    roles = [db.session.query(Role).filter_by(id=role_id).first() for role_id in role_ids]
    names = [role.name for role in roles]
    if "coordinator" in names:
      return True
    else:
      return False

  def get_role(self):
    user_role = db.session.query(UserRoles).filter_by(user_id=self.id).first()
    role = db.session.query(Role).filter_by(id=user_role.role_id).first()
    return role

  def get_user_role(self):
    role = db.session.query(UserRoles).filter_by(user_id=self.id).first()
    return role

  def is_mentor(self):
    role_ids = [user_role.role_id for user_role in db.session.query(UserRoles).filter_by(user_id=self.id).all()]
    roles = [db.session.query(Role).filter_by(id=role_id).first() for role_id in role_ids]
    names = [role.name for role in roles]
    if 'mentor' in names:
      return True
    else:
      return False

  def get_defaults(self):
    return {'id': self.id,
        'username': self.username,
        'email': self.email,
        'first_name': self.first_name,
        'last_name': self.last_name,
        'role_id': unicode(str(self.get_role().id), 'utf-8')
        }

  def update_all(self, username, email, first_name, last_name):
    self.username = username
    self.email = email
    self.first_name = first_name
    self.last_name = last_name

  def can_view_student(self, student_id):
    role_name = self.get_role().name
    student = db.session.query(Student).filter_by(id=student_id).first()
    if role_name == 'admin':
      return True
    elif role_name == 'mentor':
      return student.mentor_id == self.id
    elif role_name == 'coordinator':
      school = db.session.query(School).filter_by(id=student.school_id).first()
      if school is None:
        return False
      count = db.session.query(SchoolCoordinatorJoin).filter(SchoolCoordinatorJoin.school_id==school.id, SchoolCoordinatorJoin.user_id==self.id).count()
      return count > 0
    else:
      return False

  def can_view_school(self, school_id):
    role_name = self.get_role().name
    school = db.session.query(School).filter_by(id=school_id).first()
    if role_name == 'admin':
      return True
    elif role_name == 'coordinator':
      count = db.session.query(SchoolCoordinatorJoin).filter(SchoolCoordinatorJoin.school_id==school_id, SchoolCoordinatorJoin.user_id==self.id).count()
      return count > 0
    else:
      return False

  def can_view_response(self, response_id):
    response = db.session.query(SurveyResponse).filter_by(id=response_id).first()
    student = response.student
    return self.can_view_student(student.id)


db_adapter = SQLAlchemyAdapter(db, User)
user_manager = UserManager(db_adapter, app)

class Role(db.Model):
  id = db.Column(db.Integer, primary_key=True)
  name = db.Column(db.String(50), unique=True)

class UserRoles(db.Model):
  id = db.Column(db.Integer, primary_key=True)
  user_id = db.Column(db.Integer(), db.ForeignKey('user.id', ondelete="CASCADE"))
  role_id = db.Column(db.Integer(), db.ForeignKey('role.id', ondelete="CASCADE"))

class Teacher(db.Model):
  __tablename__ = "teachers"
  id = db.Column(db.Integer, primary_key=True)
  name = db.Column(db.String(255))
  students = db.relationship('Student', backref='teachers', lazy='dynamic')
  fieldtrips = db.relationship('FieldTrip', backref='teachers', lazy='dynamic')
  school_id = db.Column(db.Integer, db.ForeignKey("schools.id"))

  def __init__(self, name, school_id):
    self.name = name
    self.school_id = school_id

  def get_school_name(self):
    school = db.session.query(School).filter_by(id=self.school_id).first()
    if school is not None:
      return school.name
    else:
      return None

  def get_student_count(self):
    return db.session.query(Student).filter_by(teacher_id=self.id).count()

class Student(db.Model):
  __tablename__ = "students"
  id = db.Column(db.Integer, primary_key=True)
  first_name = db.Column(db.String(128))
  last_name = db.Column(db.String(128))
  grade = db.Column(db.Integer)
  teacher_id = db.Column(db.Integer, db.ForeignKey("teachers.id"))
  gender = db.Column(db.String(1))
  aaft = db.Column(db.Boolean)
  mentoring = db.Column(db.Boolean)
  school_id = db.Column(db.Integer, db.ForeignKey("schools.id"))
  mentor_id = db.Column(db.Integer, db.ForeignKey("user.id"))
  field_trip_events = db.relationship('FieldTripEvent', backref='attendee', lazy='dynamic')
  surveys = db.relationship('SurveyResponse', backref='student', lazy='dynamic')
  answers = db.relationship('Answer', backref='answerer', lazy='dynamic')

  def __init__(self, first_name, last_name, grade, teacher_id, gender, aaft=False, mentoring=False, school_id=None, mentor_id=None):
    self.first_name = first_name
    self.last_name = last_name
    self.grade = int(grade)
    self.teacher_id = teacher_id
    self.gender = gender
    self.aaft = aaft
    self.mentoring = mentoring
    self.school_id = school_id
    self.mentor_id = mentor_id

  def update_all(self, first_name, last_name, grade, teacher_id, gender, aaft, mentoring, school_id, mentor_id):
    self.first_name = first_name
    self.last_name = last_name
    self.grade = int(grade)
    self.teacher_id = teacher_id
    self.gender = gender
    self.aaft = aaft
    self.mentoring = mentoring
    self.school_id = school_id
    self.mentor_id = mentor_id

  def get_identifying_info(self):
    string = "%s %s, %s" % (self.first_name, self.last_name, self.get_teacher_name())
    return string

  def get_school_name(self):
    school = db.session.query(School).filter_by(id=self.school_id).first()
    if school is not None:
      return school.name
    else:
      return 'None'

  def get_teacher_name(self):
    teacher = db.session.query(Teacher).filter_by(id=self.teacher_id).first()
    if teacher is not None:
      return teacher.name
    else:
      return 'None'

  def get_mentor_name(self):
    mentor = db.session.query(User).filter_by(id=self.mentor_id).first()
    if mentor is not None:
      return '%s %s' % (mentor.first_name, mentor.last_name)
    else:
      return 'None'

  def get_field_trip_count(self):
    field_trip_events = db.session.query(FieldTripEvent).filter_by(student_id=self.id).all()
    return len(field_trip_events)

  def get_survey_count(self):
    survey_responses = db.session.query(SurveyResponse).filter_by(student_id=self.id).all()
    return len(survey_responses)

  def is_on_roster(self, field_trip_id):
    count = db.session.query(FieldTripEvent).filter(FieldTripEvent.student_id==self.id, FieldTripEvent.field_trip_id==field_trip_id).count()
    if count >= 1:
      return True
    else:
      return False

  def get_defaults(self):
    return {'id': self.id,
        'first_name': self.first_name,
        'last_name': self.last_name,
        'grade': str(self.grade),
        'teacher_id': self.teacher_id,
        'gender': self.gender,
        'aaft': '0' if self.aaft == False else '1',
        'mentoring': '0' if self.mentoring == False else '1',
        'school': str(self.school_id),
        'mentor': str(self.mentor_id)
        }

class FieldTrip(db.Model):
  __tablename__ = "field_trips"
  id = db.Column(db.Integer, primary_key=True)
  date = db.Column(db.DateTime)
  venue_id = db.Column(db.Integer, db.ForeignKey("field_trip_venues.id"))
  teacher_id = db.Column(db.Integer, db.ForeignKey("teachers.id"))
  survey = db.relationship('Survey', backref='fieldtrip', lazy='dynamic')

  def __init__(self, venue_id, date, teacher_id):
    self.date = date
    self.venue_id = venue_id
    self.teacher_id = teacher_id

  def get_identifying_info(self):
    venue = db.session.query(FieldTripVenue).filter_by(id=self.venue_id).first()
    string = "%s, %s" % (venue.name, self.date.strftime('%x'))
    return string

  def get_venue_name(self):
    return db.session.query(FieldTripVenue).filter_by(id=self.venue_id).first().name

  def get_student_count(self):
    events = db.session.query(FieldTripEvent).filter_by(field_trip_id=self.id).all()
    student_ids = [event.student_id for event in events]
    return len(set(student_ids))


# FieldTrip has FieldTripVenue
class FieldTripVenue(db.Model):
  __tablename__ = "field_trip_venues"
  id = db.Column(db.Integer, primary_key=True)
  name = db.Column(db.String(255))
  field_trips = db.relationship('FieldTrip', backref='venue', lazy='dynamic')

  def __init__(self, name):
    self.name = name

# Student has FieldTripEvent
# FieldTrip has FieldTripEvent
class FieldTripEvent(db.Model):
  __tablename__ = "field_trip_events"
  id = db.Column(db.Integer, primary_key=True)
  student_id = db.Column(db.Integer, db.ForeignKey("students.id", ondelete='CASCADE'))
  field_trip_id = db.Column(db.Integer, db.ForeignKey("field_trips.id", ondelete='CASCADE'))

  def __init__(self, student_id, field_trip_id):
    self.student_id = student_id
    self.field_trip_id = field_trip_id

  def get_field_trip(self):
    ft = db.session.query(FieldTrip).filter_by(id=self.field_trip_id).first()
    return ft

class School(db.Model):
  __tablename__ = "schools"
  id = db.Column(db.Integer, primary_key=True)
  name = db.Column(db.String(255), unique=True)
  students = db.relationship('Student', backref='school', lazy='dynamic')
  teachers = db.relationship('Teacher', lazy='dynamic')
  # teacher_id = db.Column(db.Integer, db.ForeignKey("teachers.id"))
  coordinator_joins = db.relationship('SchoolCoordinatorJoin', backref='ownedschool', lazy='dynamic')

  def __init__(self, name):
    self.name = name

  def get_defaults(self):
    return {
      'id': self.id,
      'name': self.name
    }

  def get_teachers_names(self):
    teachers = db.session.query(Teacher).filter_by(school_id=self.id).all()
    names = [t.name for t in teachers]
    if not names:
      return None
    else:
      return names

  def get_teacher_count(self):
    return db.session.query(Teacher).filter_by(school_id=self.id).count()

  def get_student_count(self):
    return db.session.query(Student).filter_by(school_id=self.id).count()

  def update_all(self, name):
    self.name = name

class SchoolCoordinatorJoin(db.Model):
  __tablename__ = 'school_coordinator_join'
  id = db.Column(db.Integer, primary_key=True)
  school_id = db.Column(db.Integer, db.ForeignKey("schools.id", ondelete='CASCADE'))
  user_id = db.Column(db.Integer, db.ForeignKey("user.id", ondelete='CASCADE'))

  def __init__(self, school_id, user_id):
    self.school_id = school_id
    self.user_id = user_id

class Survey(db.Model):
  __tablename__ = "surveys"
  id = db.Column(db.Integer, primary_key=True)
  is_mentoring_survey = db.Column(db.Boolean)
  name = db.Column(db.String(255))
  survey_questions = db.relationship('SurveyQuestion', backref='survey', lazy='dynamic')
  survey_responses = db.relationship('SurveyResponse', backref='source_survey', lazy='dynamic')
  field_trip_id = db.Column(db.Integer, db.ForeignKey("field_trips.id"))

  def __init__(self, name, is_mentoring_survey, field_trip_id=None):
    self.name = name
    self.is_mentoring_survey = is_mentoring_survey
    self.field_trip_id = field_trip_id

  def get_defaults(self):
    return {'id': self.id,
        'name': self.name,
        'field_trip_id': str(self.field_trip_id),
        'is_mentoring': str(self.is_mentoring_survey)
        }

  def update_all(self, name, is_mentoring, field_trip_id):
    self.name = name
    self.is_mentoring_survey = is_mentoring
    self.field_trip_id = field_trip_id

class Question(db.Model):
  __tablename__ = "questions"
  id = db.Column(db.Integer, primary_key=True)
  text = db.Column(db.String(255))
  question_type = db.Column(db.String(255))
  survey_questions = db.relationship('SurveyQuestion', backref='source_question', lazy='dynamic')

  def __init__(self, text, type):
    self.text = text
    self.question_type = type

  def get_defaults(self):
    return {
      'id': self.id,
      'text': self.text,
      'type': self.question_type
    }

  def update_all(self, text):
    self.text = text

  def get_question_text(self):
    return self.text

class SurveyQuestion(db.Model):
  __tablename__ = "survey_questions"
  id = db.Column(db.Integer, primary_key=True)
  survey_id = db.Column(db.Integer, db.ForeignKey("surveys.id", ondelete='CASCADE'))
  question_id = db.Column(db.Integer, db.ForeignKey("questions.id"))
  answer_choices = db.relationship('SurveyQuestionAnswer', backref='owning_question', lazy='dynamic')
  metric_id = db.Column(db.Integer, db.ForeignKey("metrics.id"))

  def __init__(self, survey_id, question_id):
    self.survey_id = survey_id
    self.question_id = question_id

  def get_question_text(self):
    question = db.session.query(Question).filter_by(id=self.question_id).first()
    return question.text

  def get_answer_choices(self):
    answers = db.session.query(SurveyQuestionAnswer).filter_by(question_id=self.id).all()
    return answers

  def get_identifying_name(self):
    string = "survey_%d_question_%d_%s" % (self.survey_id, self.question_id, self.source_question.question_type)
    return string

  def get_question_type(self):
    return self.source_question.question_type

class SurveyQuestionAnswer(db.Model):
  __tablename__ = "survey_question_answers"
  id = db.Column(db.Integer, primary_key=True)
  question_id = db.Column(db.Integer, db.ForeignKey("survey_questions.id", ondelete='CASCADE'))
  preset_answer_id = db.Column(db.Integer, db.ForeignKey("preset_answers.id"))
  answers = db.relationship('Answer', backref='answer_choice', lazy='dynamic')

  def __init__(self, question_id, preset_answer_id):
    self.question_id = question_id
    self.preset_answer_id = preset_answer_id

  def get_answer_value(self):
    return self.source_answer.answer_value

  def get_answer_text(self):
    return self.source_answer.answer_text


class PresetAnswer(db.Model):
  __tablename__ = "preset_answers"
  id = db.Column(db.Integer, primary_key=True)
  answer_choices = db.relationship('SurveyQuestionAnswer', backref='source_answer', lazy='dynamic')
  answer_text = db.Column(db.String(255))
  answer_value = db.Column(db.Integer)

  def __init__(self, answer_text, answer_value):
    self.answer_text = answer_text
    self.answer_value = answer_value

  # function to enable editing
  def get_defaults(self):
    return {
      'id': self.id,
      'answer_text': self.answer_text,
      'answer_value': self.answer_value,
    }

  def update_all(self, text, value):
    self.answer_text = text
    self.answer_value = value

class Answer(db.Model):
  __tablename__ = "answers"
  id = db.Column(db.Integer, primary_key=True)
  survey_question_answer_id = db.Column(db.Integer, db.ForeignKey("survey_question_answers.id", ondelete='CASCADE'))
  student_id = db.Column(db.Integer, db.ForeignKey("students.id"))
  question_id = db.Column(db.Integer, db.ForeignKey("questions.id", ondelete='CASCADE'))
  survey_response_id = db.Column(db.Integer, db.ForeignKey("survey_responses.id", ondelete='CASCADE'))
  other_answer_text = db.Column(db.Text())

  def __init__(self, student_id, question_id, survey_response_id, survey_question_answer_id=None, other_answer_text=None):
    self.survey_question_answer_id = survey_question_answer_id
    self.sqa = db.session.query(SurveyQuestionAnswer).filter_by(id=self.survey_question_answer_id).first() if self.survey_question_answer_id else None
    self.survey_response_id = survey_response_id
    self.student_id = student_id
    self.question_id = question_id
    self.other_answer_text = other_answer_text

  def get_question_text(self):
    sq = db.session.query(Question).filter_by(id=self.question_id).first()
    return sq.get_question_text()

  def get_answer_text(self):
    if self.survey_question_answer_id:
      sqa = db.session.query(SurveyQuestionAnswer).filter_by(id=self.survey_question_answer_id).first()
      pa = db.session.query(PresetAnswer).filter_by(id=sqa.preset_answer_id).first()
      return pa.answer_text
    else:
      return self.other_answer_text

  def get_answer_value(self):
    if self.survey_question_answer_id:
      sqa = db.session.query(SurveyQuestionAnswer).filter_by(id=self.survey_question_answer_id).first()
      pa = db.session.query(PresetAnswer).filter_by(id=sqa.preset_answer_id).first()
      return pa.answer_value
    else:
      return None

  def get_question_metric(self):
    sqa = db.session.query(SurveyQuestionAnswer).filter_by(id=self.survey_question_answer_id).first()
    if not sqa:
      return None
    sq = db.session.query(SurveyQuestion).filter_by(id=sqa.question_id).first()
    metric_id = sq.metric_id
    metric = db.session.query(Metric).filter_by(id=metric_id).first()
    if metric:
      return metric.name
    else:
      return None

  def get_question_type(self):
    question = db.session.query(Question).filter_by(id=self.question_id).first()
    return question.question_type

class SurveyResponse(db.Model):
  __tablename__ = "survey_responses"
  id = db.Column(db.Integer, primary_key=True)
  student_id = db.Column(db.Integer, db.ForeignKey("students.id", ondelete='CASCADE'))
  date = db.Column(db.DateTime)
  answers = db.relationship("Answer", backref="owning_survey", lazy='dynamic')
  survey_id = db.Column(db.Integer, db.ForeignKey("surveys.id", ondelete='CASCADE'))

  def __init__(self, survey_id, student_id):
    self.survey_id = survey_id
    self.student_id = student_id
    self.date = datetime.datetime.now()

  def calculate_metrics(self):
    obj = {}
    metrics = db.session.query(Metric).all()
    for metric in metrics:
      answers = db.session.query(Answer).filter(Answer.survey_response_id == self.id).all()
      answers = [a for a in answers if a.get_question_metric() == metric.name]
      values = [a.get_answer_value() for a in answers]
      if len(values) == 0:
        obj[metric.name] = {
          "average": 0,
          "data": []
        }
      else:
        text_val_pairs = [(a.get_answer_text(), a.get_answer_value()) for a in answers]
        nvalues = []
        for v in values:
          nvalues.append(v) if v is not None else nvalues.append(0)
        average = float(sum(nvalues))/len(nvalues)
        obj[metric.name] = {
          "average": average,
          "data": []
        }
        for text, val in text_val_pairs:
          obj[metric.name]["data"].append((text, val))
    return obj

  def serialize_without_metrics(self):
    obj = {}
    obj['id'] = self.id
    answers = db.session.query(Answer).filter(Answer.survey_response_id==self.id).all() #get all answers related to this particular survey response
    for answer in answers:
      obj[answer.get_question_text()] = answer.get_answer_text()
    return obj

  def serialize_with_metrics(self): 
    obj = self.serialize_without_metrics()
    obj['metrics'] = self.calculate_metrics()
    return obj

class Metric(db.Model):
  __tablename__ = "metrics"
  id = db.Column(db.Integer, primary_key=True)
  name = db.Column(db.String(255))
  questions = db.relationship("SurveyQuestion", backref="metric", lazy="dynamic")

  def __init__(self, name):
    self.name = name

  def calculate_survey_average(self, survey_id):
    responses = db.session.query(SurveyResponse).filter(SurveyResponse.survey_id == survey_id).all()
    if len(responses) == 0:
      return 0.0
    else:
      averages = [self.calculate_response_average(response.id) for response in responses]
      return round(float(sum(averages))/len(responses), 1)

  def calculate_response_average(self, survey_response_id):
    answers = db.session.query(Answer).filter(Answer.survey_response_id == survey_response_id).all()
    answers = [a for a in answers if a.get_question_metric() == self.name]
    values = [a.get_answer_value() for a in answers]
    if len(values) == 0:
      return 0.0
    else:
      return float(sum(values))/len(values)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
