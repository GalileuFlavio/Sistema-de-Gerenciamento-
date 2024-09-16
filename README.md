# Sistema-de-Gerenciamento-
Sistema de Gerenciamento de Projetos

from sqlalchemy import create_engine, Column, Integer, String, ForeignKey, Date
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import relationship, sessionmaker

engine = create_engine('sqlite:///projetos.db')
Base = declarative_base()

class Projeto(Base):
    __tablename__ = 'projetos'
    id = Column(Integer, primary_key=True)
    nome = Column(String)
    data_inicio = Column(Date)
    data_fim = Column(Date)
    status = Column(String)

class Tarefa(Base):
    __tablename__ = 'tarefas'
    id = Column(Integer, primary_key=True)
    descricao = Column(String)
    projeto_id = Column(Integer, ForeignKey('projetos.id'))
    projeto = relationship("Projeto", back_populates="tarefas")

Projeto.tarefas = relationship("Tarefa", order_by=Tarefa.id, back_populates="projeto")

Base.metadata.create_all(engine)
Session = sessionmaker(bind=engine)
session = Session()
