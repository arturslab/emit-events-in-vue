<template>
    <div>
        <h1>
            How to Emit Data between components in Vue
        </h1>
        <p>
            The Vue application is designed to demonstrate how to send events and props between sibling components using
            events and props without using Vuex. I will show two solutions: one using event emission through the parent
            component and the other using event emission from one component to another using root emit. The diagram
            illustrates the chosen method of communication between components.
        </p>
        <p>
            The application has three components: <strong>Child Component One</strong>, <strong>Child Component
                Two</strong>, and
            <strong>Child Component Three</strong>. Each component has a <code>"Reload"</code> button that runs a method
            to
            emit an event to the parent component and the parent component updates the <code>reload</code> status of the
            target component in the <code>tabItems</code> array. The <code>reload</code> status of each component is
            used
            to trigger the reload of the component. The component will display a message <em>"Reload component
                <span v-pre> {{ title }} </span> in progress..."</em> for 2 seconds before displaying the
            content of the component.
        </p>
        <p>
            The application also has a <code>tabItems</code> array that contains the details of each component. The
            <code>tabItems</code> array is used to track the reload status of each component. When a component is
            reloaded,
            the <code>reload</code> property of the component is set to true. The <code>onComponentChange</code> event
            is
            emitted when the "Reload" button of a component is clicked. The <code>onComponentChange</code> event
            contains
            the
            <code>reload</code> status and the target component to reload. The <code>onComponentChange</code> event is
            handled
            in the parent component (<code>Home.vue</code>) to update the <code>reload</code> status of the target
            component
            in the <code>tabItems</code> array. The <code>reload</code> status of each component is used to trigger the
            reload
            of the component.
        </p>

        <strong>Instructions:</strong>
        <ol>
            <li>
                Click on the "Reload" button of any component to reload that
                component.
            </li>
            <li>
                The component will reload and display a message "Reload component
                <span v-pre> {{ title }} </span> in progress..." for 2 seconds.
            </li>
            <li>
                After 2 seconds, the component will display the content of the
                component.
            </li>
        </ol>

    </div>
</template>

<script>
export default {
    name: "ComponentIntro",
};
</script>