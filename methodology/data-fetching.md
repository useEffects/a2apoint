## Optimizing Data Fetching in Next.js and Expo

In the last chapter, we discussed how to share a single codebase for Next.js and Expo. Now, let's explore how to utilize the beneficial features that each framework offers, including Next.js's server components and incremental static generation, and `@tanstack/react-query` in Expo for offline capabilities.

### Design Choices

Consider the following piece of code:

```tsx
export default function Foobar({ id }: { id: string }) {
    const [data, setData] = useState(null);
    useEffect(() => {
        fetchData(id).then(setData);
    }, [id]);

    return data && (
        <View>
            <Hamburger data={data} />
        </View>
    );
}
```

We consider this an anti-pattern in our design because initial data fetching is tightly coupled with the UI. To leverage the best features of each framework, we handle data fetching separately.

The code then becomes:

```tsx
export default function Foobar({ initialData }: { initialData: Data }) {
    return <Hamburger data={initialData} />;
}
```

## Next.js: Server Components and Incremental Static Generation

Next.js v13+ introduces the `app` router, allowing us to use [React Server Components](https://react.dev/reference/rsc/server-components). This enables building blazingly fast pre-rendered pages. By using `generateStaticParams` and Incremental Static Generation, we can optimize our application for performance.

#### Example

```tsx
// apps/next/src/app/foobar/page.tsx
import { Suspense } from 'react';
import { fetchData, fetchAllData } from 'app/lib/data-fetching';
import Foobar from 'app/screens/foobar';

export default function FoobarPage({ params: { id } }: { params: { id: string } }) {
    const data = fetchData(id);

    return (
        <Suspense fallback={<div>Loading...</div>}>
            <Foobar initialData={data} />
        </Suspense>
    );
}

// generateStaticParams example
export async function generateStaticParams() {
    const data = await fetchAllData();
    return data.map(item => ({ id: item.id }));
}
```

## Building Offline Apps in Expo with React Query

Using `@tanstack/react-query` in Expo, we can cache initial data in local storage and set a stale time of at least a week. This makes it excellent for building offline apps. Note: Data fetching after user interactions, like clicking a button or submitting a form, can be handled directly within the component itself.

#### Example

```tsx
// apps/expo/app/foobar/index.tsx
import { useQuery } from '@tanstack/react-query';
import Foobar from 'app/screens/foobar';
import { fetchData } from 'app/lib/data-fetching';

export default function FoobarScreen() {
    const { id } = useParams<{ id: string }>();
    const { data, isLoading } = useQuery({
        queryKey: [id],
        queryFn: () => fetchData(id),
        initialData: [],
        staleTime: 1000 * 60 * 60 * 24 * 7, // 1 week
        gcTime: 1000 * 60 * 60 * 24 * 7, // 1 week
    });

    if (isLoading) return <View><Text>Loading...</Text></View>;

    return <Foobar initialData={data} />;
}
```