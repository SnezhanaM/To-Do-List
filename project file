from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy import Column, Integer, String, Date
from datetime import datetime, timedelta
from sqlalchemy.orm import sessionmaker

#create datebase file
engine = create_engine('sqlite:///todo.db?check_same_thread=False')

Base = declarative_base() #parent class


class Table(Base):
    __tablename__ = 'task'
    id = Column(Integer, primary_key=True)
    task = Column(String, default='default_value')
    deadline = Column(Date, default=datetime.today())

    def __repr__(self):
        return self.task

#Base.metadata.drop_all(engine)
Base.metadata.create_all(engine) #create table in database

Session = sessionmaker(bind=engine)
session = Session()
#The session object is the only thing you need to manage the database

while True:
    print("""1) Today's tasks
2) Week's tasks
3) All tasks
4) Missed tasks
5) Add task
6) Delete task
0) Exit""")
    user_input = int(input())
    if user_input == 1:
        today = datetime.today().date()
        rows = session.query(Table).filter(Table.deadline == today).all()
        if not rows:
            print(f"""\nToday {today.day} {today.strftime('%b')}:
Nothing to do!\n""")
        else:
            print(f"\nToday {today.day} {today.strftime('%b')}:")
            for el in rows:
                print(el.id, '. ', el.task, '\n', sep='')
    elif user_input == 2:
        print()
        for day in range(7):
            today = datetime.today().date() + timedelta(days=day)
            print(f"{today.strftime('%A')} {today.day} {today.strftime('%b')}")
            rows = session.query(Table).filter(Table.deadline == today).all()
            if not rows:
                print('Nothing to do!')
            else:
                for el in rows:
                    print(el.id, '. ', el.task, sep='')
            print()
    elif user_input == 3:
        print("\nAll tasks:")
        rows = session.query(Table).all()
        for el in rows:
            print(el.id, '. ', el.task, ' ', el.deadline, sep='')
        print()
    elif user_input == 4:
        today = datetime.today().date()
        rows = session.query(Table).filter(Table.deadline < today).all()
        print('\nMissed tasks:')
        if not rows:
            print('Nothing is missed!')
        else:
            for el in rows:
                print(el.id, '. ', el.task, ' ', el.deadline, sep='')
        print()
    elif user_input == 5:
        print('\nEnter task')
        user_task = input()
        print('Enter deadline')
        user_deadline = input()
        #table_time = datetime.strptime(user_deadline, '%Y-%m-%d')
        new_row = Table(task=user_task,
                        deadline=datetime.strptime(user_deadline, '%Y-%m-%d').date())
        session.add(new_row)
        session.commit()
        print('The task has been added!\n')
    elif user_input == 6:
        print('\nChose the number of the task you want to delete:')
        today = datetime.today().date()
        rows = session.query(Table).all()
        if not rows:
            print('Nothing to delete\n')
        else:
            for el in rows:
                print(el.id, '. ', el.task, ' ', el.deadline, sep='')
        user_delete = int(input())
        session.delete(rows[user_delete])
        session.commit()
        print('The task has been deleted!\n')
    else:
        print('\nBye!')
        break
