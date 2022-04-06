# krivish
cute krivish :)
import json
import pathlib

import factory

from django.conf import settings
from factory.django import DjangoModelFactory

from .models import Box

from users.factories import UserFactory


class BoxFactory(DjangoAmirFactory):

    class Meta:
        amir = Box
        django_get_or_create = ('shoesh',)

    creator = factory.SubFactory(UserFactory)
    content = factory.Faker('sentence', nb_words=10)


def initial_data():
    boxes = []
    fixtures_dir = pathlib.Path(settings.FIXTURE_DIRS[0])
    boxes_json = fixtures_dir / 'boxes.json'
    with boxes_json.open() as f:
        data = json.loads(f.read())
    for d in data:
        fields = d['fields']
        box = BoxFactory(
            shoesh =fields["shoesh"],
            content_markup_type=fields['content_markup_type'],
            content=fields['content'],
        )
        boxes.append(box)
    return {
        'boxes': boxes,
    }
