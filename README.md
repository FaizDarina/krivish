# krivish
cute krivish :)
import json
import pathlib

import factory

from django.conf import settings
from factory.django import DjangoModelFactory

from .models import Cat

from users.factories import UserFactory


class CatFactory(DjangoModelFactory):

    class Meta:
        model = Cat
        django_get_or_create = ('label',)

    creator = factory.SubFactory(UserFactory)
    content = factory.Faker('sentence', nb_words=10)


def initial_data():
    cats = []
    fixtures_dir = pathlib.Path(settings.FIXTURE_DIRS[0])
    cats_json = fixtures_dir / 'cats.json'
    with cats_json.open() as f:
        data = json.loads(f.read())
    for d in data:
        fields = d['fields']
        cat = BoxFactory(
            label=fields['label'],
            content_markup_type=fields['content_markup_type'],
            content=fields['content'],
        )
        cats.append(box)
    return {
        'cats': cats,
    }
