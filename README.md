# WebWeek4
import React from "react";
import Select, { InputActionMeta } from "react-select";
import { Option } from "@/types";
import { FormControl, FormHelperText, Box, BoxProps } from "@material-ui/core";
import { StyledIcon, StyledBox, customStyles } from "./Styled";

interface Props {
  options: Option[];
  onChange: (key: string, value: Option | Option[] | null | undefined) => void;
  name: string;
  value: Option | Option[] | null | undefined;
  placeholder?: string;
  isMulti?: boolean;
  isLoading?: boolean;
  isDisabled?: boolean;
  boxprops?: BoxProps;
  maxMenuHeight?: number;
  label?: string | React.ReactNode;
  labelPlacement?: "top" | "left" | "right" | "bottom";
  error?: {
    message: string;
  };
  defaultValue?: Option | (Option | undefined)[] | undefined;
  required?: boolean;
  onInputChange?: (newValue: string, actionMeta: InputActionMeta) => void;
  styles?: object;
  components?: object;
  filterOption?: () => boolean;
  onFocus?: () => void;
}

const DropdownIndicator: React.FC<{ hasValue: boolean }> = () => <StyledIcon />;

const defaultComponents = {
  IndicatorSeparator: () => null,
  DropdownIndicator,
};

const MultiSelect: React.FC<Props> = ({
  isMulti,
  name,
  onChange,
  options,
  placeholder,
  isLoading,
  isDisabled,
  value,
  boxprops,
  maxMenuHeight,
  error,
  label,
  labelPlacement = "top",
  defaultValue,
  required,
  onInputChange,
  filterOption,
  onFocus,
  ...props
}) => {
  const handleChange = (value: any) => {
    onChange(name, value);
  };

  const boxWrapper = {
    top: {},
    left: { display: "flex", alignItems: "center" },
  };
  const inputWrapper = {
    top: { marginTop: "4px" },
    left: { flex: 1, marginLeft: "10px", minWidth: "150px" },
  };

  const components = {
    ...defaultComponents,
    ...props.components,
  };

  const styles = {
    ...customStyles,
    ...props.styles,
  };

  return (
    <StyledBox {...boxprops} className={`hreactselect__wrapper input-${name}__wrapper`}>
      <FormControl error={!!error} fullWidth>
        <Box {...boxWrapper[labelPlacement]}>
          {label && (
            <label htmlFor={name}>
              {!!required && <span className="required">* </span>}
              {label}
            </label>
          )}
          <Box {...inputWrapper[labelPlacement]}>
            <Select
              onInputChange={onInputChange}
              styles={styles}
              isMulti={isMulti}
              options={options}
              components={components}
              onChange={handleChange}
              placeholder={placeholder}
              isLoading={isLoading}
              value={value}
              isDisabled={isDisabled}
              defaultValue={defaultValue}
              maxMenuHeight={maxMenuHeight || 250}
              className={error ? "Mui-error" : ""}
              menuPlacement="auto"
              isOptionDisabled={(option) => option?.disabled === true}
              filterOption={filterOption}
              onFocus={onFocus}
            />
            {!!error && <FormHelperText> {error.message}</FormHelperText>}
          </Box>
        </Box>
      </FormControl>
    </StyledBox>
  );
};

export default MultiSelect;
