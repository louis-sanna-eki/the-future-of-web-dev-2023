---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: images/blue-circle.png
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# persist drawings in exports and build
drawings:
  persist: false
# page transition
transition: fade-out
# use UnoCSS
css: unocss
download: https://louis-sanna-eki.github.io/front-unit-tests-slides/export/unit_testing_on_the_frontend.pdf
favicon: images/eki-favicon.png
---

# Unit testing on the frontend

Extract the state, test the state

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Press Space for next page <carbon:arrow-right class="inline"/>
  </span>
</div>



---
layout: image-right
image: images/yellow-circle.png
---

# Table of contents

- Hexagonal architecture on the front-end
- Separating the state for rest
- Testing the store
- Presentational component vs container component
- Testing librairies for react: Vitest, React Testing Library, Mock Server Worker
- Links

---
layout: two-cols
---

# Hexagonal architecture

<img src="/images/clean-architecture.png" class="h-80 rounded shadow" />

::right::

<ul class="pl-8 mt-16">
<li>Hexagonal architecture/Clean architecture/onion architecture: same idea under many names<br/></li>
<li>Domain logic should be in it's own dedicated layer<br/></li>
<li>Domain layer does not depend on any other layer (may demand dependency inversion)<br/></li>
<li>Ports/adapters pattern that let's change other layer easily (SQL to elascticSearch, REST to graphQL, etc...)<br/></li>
</ul>

---
---

# Clean architecture on the backend


```ts {all|1|2-6|9|all}
async function sendFeedbackController(request: Request, response: Response): Promise<void> {
  const feedback = z
    .object({
      payload: z.any(),
      type: feedbackTypeSchema,
    })
    .parse(request.body);
  const user = parseAuthenticatedUser(response);
  await sendFeedback({ feedback, user });
  response.send({});
}
```

- On the backend, we remove request handling and db logic from domain services (handlers and repositories)
- See above the handler above that parse the request, calls a domain service, then send the response

---
---

# Repository pattern: makes database change much easier

<br/>
```ts {all|1|2-4|5-9|all}
async function getReviewsByRating({ reviewFilters} : { reviewFilters: ReviewFilters }): Promise<Aggs[]> {
  if (elasticRepository.hasReviewIndex()) {
    return elasticRepository.aggregateReviewsByRating({ reviewFilters });
  }
  return SQLrepository.aggregateReviews({
    reviewFilters,
    attributes: ['rating', [Sequelize.literal('COUNT(DISTINCT(reviews.id))'), 'count']],
    group: ['rating'],
  });
}
```

- Repository pattern: create a dedicated module to handle db logic. Quite heavy, but lets us switch from one database to another easily.
- Remark: this does NOT respect Clean Architecture by the letter. Dependency inversion wasn't done, and the SQL syntax leaks out of the SQL repository.

---
---

# Hexagonal on the front-end: the UI as an afterthought


<div class="grid grid-cols-2 gap-4">
<div>
<Tweet id="1092424711655567360" scale="0.65" />
</div>
<div>

- The core idea: remove all UI and query from the domain logic. Imagine that there are two ways to interact with app, UI or CLI. The core logic should not be duplicated.
- All HTTP calls (Web sockets, ...) should be extracted to dedicated services
- UI interfaces extracted to presentational components
- So in practice the domain logic ends up identified to with the application global state (WARNING: this may an abuse of language)

</div>
</div>

---
---

# Hexagonal on the front-end: the feedback module example

<div class="grid grid-cols-2 gap-4">
  <div>
    <img src="/images/feedback-modal.png" class="h-80 rounded shadow" />
  </div>
  <div class="pl-12">
    <img src="/images/feedback-module.png" class="h-80 rounded shadow" />
  </div>
</div>

---
---

# The feedback module example: an HTTP service

<br/>

```ts
function useNewFeedback({
  options = {},
}: {
  options?: MutationOptions<Feedback>;
}): SafeMutation<Feedback> {
  const mutationFn = usePostMutationFn({ input: `/api/feedback` });
  return usePostMutation<Feedback>({ options: { mutationFn, ...options } });
}
```

<br/>

- Service handling HTTP requests to backend

---
---

# The feedback module example: a view

```tsx {all|2|3|5-21|all}
function IncorrectBrandName(): JSX.Element {
  const { brandId, newFeedbackMutation } = useCollaborationContext();
  const [correctName, setCorrectName] = useState<string>('');
  if (brandId === null) return <></>;
  return (
    <>
      <Header title="The name is incorrect" />
      <Body>
        <TextInput
          onChange={(event) => setCorrectName(event.target.value)}
          value={correctName}
        />
        <Actions
          onMainClick={() => sendFeedback({
              type: 'incorrectBrandName',
              payload: { brandId, correctName },
            })}
        />
      </Body>
    </>
  );
}
```

<br/>

- View contains presentational logic
- It can (and should IMO) contain its own state

---
---

# The feedback module example: the store

```tsx
function useFeedbackStore({
  params = {},
  useNewFeedback,
}: FeedbackStoreProps): IFeedbackStore {
  const [isModalOpen, setIsModalOpen] = useState<boolean>(false);

  const historyStore = useFeedbackHistory({});
  const { view } = historyStore;

  const { reverse, ...navigationMethods } = useReverse(historyStore);
  const { clearHistory, goBack, goBackToStart } = navigationMethods;

  const { goTo, productId, setProductId, brandId, setBrandId } = useSelection({
    ...navigationMethods,
    params,
  });

  const newFeedbackMutation = useNewFeedback({ options: { onMutate: () => goTo('success') } });

  return { ... };
}
```

---
---

# The feedback module example: a use-case

```tsx
function useFeedbackHistory(): IFeedbackHistory {
  const [viewHistory, setViewHistory] = useState<IView[]>(['root']);

  const view = viewHistory[viewHistory.length - 1];

  const goTo = useCallback((newView) => setViewHistory((previous) => [...previous, newView]), []);
  const goBack = useMemo(() => {
    if (viewHistory.length <= 1) return null;
    return () => setViewHistory((previous) => previous.slice(0, -1));
  }, [viewHistory.length]);
  const goBackToStart = useMemo(() => {
    if (viewHistory.length <= 1) return null;
    return () => setViewHistory(['root']);
  }, [viewHistory.length]);

  return { view, goTo, goBack, goBackToStart  };
}
```

<br/>

- Navigation use-case: the user may go back to previous view, or back to start
- Use-case is extracted in its dedicated hook

---
---

# Testing the navigation use-case

```tsx
describe('useFeedbackStore', () => {
  describe('goBack', () => {
    it('should go back to previous view', () => {
      const feedbackStore = setupStore({});
      act(() => feedbackStore.goTo('missingBrand'));
      act(() => feedbackStore.goTo('success'));

      act(() => {
        if (feedbackStore.goBack === null) return;
        feedbackStore.goBack();
      });

      expect(feedbackStore.view).toEqual('missingBrand');
    });
  });
});

```

<br/>

- We test the use-case on the store of the module global state
- Remark: we did NOT test the use-case hook directly. That would could the test to the implementation, leading use to write more tests covering less code.


---
---

# Limitations

<div class="grid grid-cols-2 gap-4">
<div>
<Tweet id="1092885279256576000" scale="0.65" />
</div>
<div>

- UI is critical in the value created by our application, there is not a single CLI interface with mass-adoption. And it can be very complicated, much more complex that query handlers for instance. So it's mistake to treat UI as an implementation detail.
- Dogmatic separation of state/presentation can create artificial boundaries. Simple components can (and should) own their own state/presentation.

</div>
</div>

---
---

# Why does it work? Human/machine have different strengths


|   case/tester  | Human     |  Machine    |
| ------------   | --------- |  ---------  |
| State machine  |    Bad    |   Very good |            
| UI             | Very good |     Bad     |

<br/>

- Humans visual capabilities are very advanced. We can detect visual bugs very easily without any automated testing. But we do struggle to check that algorithms correctly implement business logic.
- The reverse is true for machines. So the highest ROI testing strategy is to extract the state/BL logic to test automatically, and check visually that the UI is correct.

---
---

# Presentational components vs container components


<img src="/images/presentation-vs-container.png" class="h-60 rounded shadow" />

<br/>

- Famous article distinguishing presentation components (mainly UI) from container components (mainly state)
- old nomenclature: smart vs dumb
- Remark: don't be dogmatic, use hooks, etc...


---
---

# Headless libraries

<br/>
Some libraries only handle the stateful portion of the components. There are called "headless" libraries.
<br/>
<br/>

<img src="/images/tanstack-table.png" class="h-40 rounded shadow m-auto" />

<br/>
See tanstack Table, which in React is a simple hook:

<br/>
<br/>
```tsx
const table = useReactTable({
  data,
  columns,
  getCoreRowModel: getCoreRowModel(),
})
```

---
---

# Testing libraries (React)

- Vitest - jest but cooler (and much faster)
<img src="/images/vitest.png" class="h-20 rounded shadow m-auto mt-2" />

- React Testing library - jsx testing made easy
<img src="/images/testing-library.png" class="h-20 rounded shadow m-auto mt-2" />

- Mock service worker - intercept HTTP calls to avoid mocking
<img src="/images/msw.png" class="h-20 rounded shadow m-auto mt-2" />


---
---


### Architecture:
- Clean architecture: https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html
- Presentational and Container Components: https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0
- UI as an afterthought: https://michel.codes/blogs/ui-as-an-afterthought
<br/>
<br/>

### Libraries:
- Mock service worker: https://mswjs.io/
- React Testing library: https://testing-library.com/
- Vitest: https://vitest.dev/
<br/>
<br/>

### React tests:
- How to test custom React hooks: https://kentcdodds.com/blog/how-to-test-custom-react-hooks
- Stop mocking fetch: https://kentcdodds.com/blog/stop-mocking-fetch

---
layout: center
---

<img src="/images/question-dog.png" with="100%" height="100%" position="absolute" top="0" left="0"/>
<div position="relative" color="#15293B" top="-200px">
<b>QUESTIONS AND REMARKS</b>
</div>

